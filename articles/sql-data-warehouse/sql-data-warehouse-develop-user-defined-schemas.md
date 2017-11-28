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
# <a name="user-defined-schemas-in-sql-data-warehouse"></a><span data-ttu-id="c0960-103">Az SQL Data Warehouse felhasználói sémák</span><span class="sxs-lookup"><span data-stu-id="c0960-103">User-defined schemas in SQL Data Warehouse</span></span>
<span data-ttu-id="c0960-104">Hagyományos adatraktárak gyakran a használata önálló adatbázisok toocreate alkalmazás határok munkaterhelés, a tartomány vagy a biztonsági alapján.</span><span class="sxs-lookup"><span data-stu-id="c0960-104">Traditional data warehouses often use separate databases toocreate application boundaries based on either workload, domain or security.</span></span> <span data-ttu-id="c0960-105">Egy hagyományos SQL Server data warehouse tartalmazhat például egy átmeneti adatbázis, adatraktár-adatbázis és néhány adat adatközpont-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="c0960-105">For example, a traditional SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="c0960-106">Ebben a topológiában az egyes adatbázisok működik, a munkaterhelés és a biztonsági határ hello architektúrában.</span><span class="sxs-lookup"><span data-stu-id="c0960-106">In this topology each database operates as a workload and security boundary in hello architecture.</span></span>

<span data-ttu-id="c0960-107">Ezzel szemben a SQL Data Warehouse hello teljes adatraktározás számítási feladatáról egy adatbázison belül futtatja.</span><span class="sxs-lookup"><span data-stu-id="c0960-107">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="c0960-108">Adatbázis közötti illesztések nem engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="c0960-108">Cross database joins are not permitted.</span></span> <span data-ttu-id="c0960-109">Ezért az SQL Data Warehouse hello adatraktár toobe hello egy adatbázisban tárolt által használt összes táblának vár.</span><span class="sxs-lookup"><span data-stu-id="c0960-109">Therefore SQL Data Warehouse expects all tables used by hello warehouse toobe stored within hello one database.</span></span>

> [!NOTE]
> <span data-ttu-id="c0960-110">Az SQL Data Warehouse nem támogatja az alhálózatok közötti adatbázis-lekérdezések bármilyen típusú.</span><span class="sxs-lookup"><span data-stu-id="c0960-110">SQL Data Warehouse does not support cross database queries of any kind.</span></span> <span data-ttu-id="c0960-111">Adatok adatraktár-megvalósítások, ezt a mintát használó következésképpen toobe módosítva lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c0960-111">Consequently, data warehouse implementations that leverage this pattern will need toobe revised.</span></span>
> 
> 

## <a name="recommendations"></a><span data-ttu-id="c0960-112">Javaslatok</span><span class="sxs-lookup"><span data-stu-id="c0960-112">Recommendations</span></span>
<span data-ttu-id="c0960-113">Ezek a javaslatok munkaterhelések, a biztonság, a tartomány és a működési határok konszolidálása felhasználó által definiált sémák használatával</span><span class="sxs-lookup"><span data-stu-id="c0960-113">These are recommendations for consolidating workloads, security, domain and functional boundaries by using user defined schemas</span></span>

1. <span data-ttu-id="c0960-114">Egy SQL Data Warehouse-adatbázis toorun használja a teljes adatraktározás számítási feladatáról</span><span class="sxs-lookup"><span data-stu-id="c0960-114">Use one SQL Data Warehouse database toorun your entire data warehouse workload</span></span>
2. <span data-ttu-id="c0960-115">A meglévő adatraktár környezet toouse egy SQL Data Warehouse-adatbázis összesítése</span><span class="sxs-lookup"><span data-stu-id="c0960-115">Consolidate your existing data warehouse environment toouse one SQL Data Warehouse database</span></span>
3. <span data-ttu-id="c0960-116">Használja ki az **felhasználói sémákkal** tooprovide hello határ korábban az adatbázisok készletével megvalósított.</span><span class="sxs-lookup"><span data-stu-id="c0960-116">Leverage **user-defined schemas** tooprovide hello boundary previously implemented using databases.</span></span>

<span data-ttu-id="c0960-117">Ha a felhasználó által definiált sémák nem korábban használt kell végrehajtania egy tiszta lappal.</span><span class="sxs-lookup"><span data-stu-id="c0960-117">If user-defined schemas have not been used previously then you have a clean slate.</span></span> <span data-ttu-id="c0960-118">Egyszerűen használni hello régi adatbázisnév hello alapjául a felhasználó által definiált sémák hello SQL Data Warehouse-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="c0960-118">Simply use hello old database name as hello basis for your user-defined schemas in hello SQL Data Warehouse database.</span></span>

<span data-ttu-id="c0960-119">Ha a séma már használták majd közül néhány:</span><span class="sxs-lookup"><span data-stu-id="c0960-119">If schemas have already been used then you have a few options:</span></span>

1. <span data-ttu-id="c0960-120">Távolítsa el a hello örökölt séma nevét, majd indítsa el a friss</span><span class="sxs-lookup"><span data-stu-id="c0960-120">Remove hello legacy schema names and start fresh</span></span>
2. <span data-ttu-id="c0960-121">Hello örökölt séma nevek előre függőben lévő hello örökölt séma neve toohello tábla neve</span><span class="sxs-lookup"><span data-stu-id="c0960-121">Retain hello legacy schema names by pre-pending hello legacy schema name toohello table name</span></span>
3. <span data-ttu-id="c0960-122">Hello örökölt séma nevek őriznie hello táblán egy extra séma toore a nézetek végrehajtási-hello régi séma struktúrát.</span><span class="sxs-lookup"><span data-stu-id="c0960-122">Retain hello legacy schema names by implementing views over hello table in an extra schema toore-create hello old schema structure.</span></span>

> [!NOTE]
> <span data-ttu-id="c0960-123">Az első ellenőrzés 3. lehetőség tűnhet hello legtöbb tetszetős lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c0960-123">On first inspection option 3 may seem like hello most appealing option.</span></span> <span data-ttu-id="c0960-124">Azonban hello ördögöt hello részletesen is.</span><span class="sxs-lookup"><span data-stu-id="c0960-124">However, hello devil is in hello detail.</span></span> <span data-ttu-id="c0960-125">Nézetek az SQL Data Warehouse csak olvasható.</span><span class="sxs-lookup"><span data-stu-id="c0960-125">Views are read only in SQL Data Warehouse.</span></span> <span data-ttu-id="c0960-126">További adatok vagy a tábla változtatások hajt végre a következő alaptáblában hello toobe lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="c0960-126">Any data or table modification would need toobe performed against hello base table.</span></span> <span data-ttu-id="c0960-127">3. lehetőség nézetek réteget is bevezeti a rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="c0960-127">Option 3 also introduces a layer of views into your system.</span></span> <span data-ttu-id="c0960-128">Érdemes lehet toogive Ez néhány további gondolat használata nézetek a architektúrában már.</span><span class="sxs-lookup"><span data-stu-id="c0960-128">You might want toogive this some additional thought if you are using views in your architecture already.</span></span>
> 
> 

### <a name="examples"></a><span data-ttu-id="c0960-129">Példák:</span><span class="sxs-lookup"><span data-stu-id="c0960-129">Examples:</span></span>
<span data-ttu-id="c0960-130">Valósítja meg a felhasználó által definiált sémák alapján az adatbázis neve</span><span class="sxs-lookup"><span data-stu-id="c0960-130">Implement user-defined schemas based on database names</span></span>

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

<span data-ttu-id="c0960-131">Tartsa meg az örökölt séma nevek által előre függőben lévő őket toohello tábla neve.</span><span class="sxs-lookup"><span data-stu-id="c0960-131">Retain legacy schema names by pre-pending them toohello table name.</span></span> <span data-ttu-id="c0960-132">Használjon sémák hello munkaterhelés határ.</span><span class="sxs-lookup"><span data-stu-id="c0960-132">Use schemas for hello workload boundary.</span></span>

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

<span data-ttu-id="c0960-133">A hagyományos séma nevek nézetek használatával</span><span class="sxs-lookup"><span data-stu-id="c0960-133">Retain legacy schema names using views</span></span>

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
> <span data-ttu-id="c0960-134">A séma stratégia változásait hello biztonsági modell áttekintése hello adatbázis szüksége van.</span><span class="sxs-lookup"><span data-stu-id="c0960-134">Any change in schema strategy needs a review of hello security model for hello database.</span></span> <span data-ttu-id="c0960-135">Sok esetben valószínűleg képesek toosimplify hello biztonsági modell hello séma szintű engedélyek meghatározásával.</span><span class="sxs-lookup"><span data-stu-id="c0960-135">In many cases you might be able toosimplify hello security model by assigning permissions at hello schema level.</span></span> <span data-ttu-id="c0960-136">Ha részletesebb engedélyek is szükségesek, majd az adatbázis-szerepkörök is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c0960-136">If more granular permissions are required then you can use database roles.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="c0960-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0960-137">Next steps</span></span>
<span data-ttu-id="c0960-138">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="c0960-138">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
