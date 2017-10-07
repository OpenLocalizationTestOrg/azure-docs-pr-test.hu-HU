---
title: "az SQL Data Warehouse aaaUser által definiált sémák |} Microsoft Docs"
description: "Tippek a Transact-SQL-sémák használata az Azure SQL Data Warehouse adattárházzal történő, megoldások."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c411d6fed68e67c444a5871eab06182eaeb6dbf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Az SQL Data Warehouse felhasználói sémák
Hagyományos adatraktárak gyakran a használata önálló adatbázisok toocreate alkalmazás határok munkaterhelés, a tartomány vagy a biztonsági alapján. Egy hagyományos SQL Server data warehouse tartalmazhat például egy átmeneti adatbázis, adatraktár-adatbázis és néhány adat adatközpont-adatbázis. Ebben a topológiában az egyes adatbázisok működik, a munkaterhelés és a biztonsági határ hello architektúrában.

Ezzel szemben a SQL Data Warehouse hello teljes adatraktározás számítási feladatáról egy adatbázison belül futtatja. Adatbázis közötti illesztések nem engedélyezettek. Ezért az SQL Data Warehouse hello adatraktár toobe hello egy adatbázisban tárolt által használt összes táblának vár.

> [!NOTE]
> Az SQL Data Warehouse nem támogatja az alhálózatok közötti adatbázis-lekérdezések bármilyen típusú. Adatok adatraktár-megvalósítások, ezt a mintát használó következésképpen toobe módosítva lesz szüksége.
> 
> 

## <a name="recommendations"></a>Javaslatok
Ezek a javaslatok munkaterhelések, a biztonság, a tartomány és a működési határok konszolidálása felhasználó által definiált sémák használatával

1. Egy SQL Data Warehouse-adatbázis toorun használja a teljes adatraktározás számítási feladatáról
2. A meglévő adatraktár környezet toouse egy SQL Data Warehouse-adatbázis összesítése
3. Használja ki az **felhasználói sémákkal** tooprovide hello határ korábban az adatbázisok készletével megvalósított.

Ha a felhasználó által definiált sémák nem korábban használt kell végrehajtania egy tiszta lappal. Egyszerűen használni hello régi adatbázisnév hello alapjául a felhasználó által definiált sémák hello SQL Data Warehouse-adatbázis között.

Ha a séma már használták majd közül néhány:

1. Távolítsa el a hello örökölt séma nevét, majd indítsa el a friss
2. Hello örökölt séma nevek előre függőben lévő hello örökölt séma neve toohello tábla neve
3. Hello örökölt séma nevek őriznie hello táblán egy extra séma toore a nézetek végrehajtási-hello régi séma struktúrát.

> [!NOTE]
> Az első ellenőrzés 3. lehetőség tűnhet hello legtöbb tetszetős lehetőséget. Azonban hello ördögöt hello részletesen is. Nézetek az SQL Data Warehouse csak olvasható. További adatok vagy a tábla változtatások hajt végre a következő alaptáblában hello toobe lenne szükség. 3. lehetőség nézetek réteget is bevezeti a rendszerbe. Érdemes lehet toogive Ez néhány további gondolat használata nézetek a architektúrában már.
> 
> 

### <a name="examples"></a>Példák:
Valósítja meg a felhasználó által definiált sémák alapján az adatbázis neve

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for hello data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in hello stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in hello edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

Tartsa meg az örökölt séma nevek által előre függőben lévő őket toohello tábla neve. Használjon sémák hello munkaterhelés határ.

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines hello data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend hello old schema name toohello table and create in hello staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend hello old schema name toohello table and create in hello data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

A hagyományos séma nevek nézetek használatával

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines hello data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines hello legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create hello base staging tables in hello staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create hello base data warehouse tables in hello data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in hello legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> A séma stratégia változásait hello biztonsági modell áttekintése hello adatbázis szüksége van. Sok esetben valószínűleg képesek toosimplify hello biztonsági modell hello séma szintű engedélyek meghatározásával. Ha részletesebb engedélyek is szükségesek, majd az adatbázis-szerepkörök is használhatja.
> 
> 

## <a name="next-steps"></a>Következő lépések
További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
