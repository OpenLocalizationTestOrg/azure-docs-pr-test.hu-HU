---
title: "aaaElastic adatbázis eszközök szószedet |} Microsoft Docs"
description: "A rugalmas adatbáziseszközöket használt kifejezések magyarázatát"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a><span data-ttu-id="a80f5-103">A rugalmas adatbázis eszközök szószedet</span><span class="sxs-lookup"><span data-stu-id="a80f5-103">Elastic Database tools glossary</span></span>
<span data-ttu-id="a80f5-104">hello következő fogalmakat a hello [skálázáshoz rugalmas adatbáziseszközöket](sql-database-elastic-scale-introduction.md), Azure SQL-adatbázis szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="a80f5-104">hello following terms are defined for hello [Elastic Database tools](sql-database-elastic-scale-introduction.md), a feature of Azure SQL Database.</span></span> <span data-ttu-id="a80f5-105">hello eszközök használt toomanage [shard leképezhető](sql-database-elastic-scale-shard-map-management.md), és hello [ügyféloldali kódtár](sql-database-elastic-database-client-library.md), hello [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md), [rugalmas készletek](sql-database-elastic-pool.md), és [lekérdezések](sql-database-elastic-query-overview.md).</span><span class="sxs-lookup"><span data-stu-id="a80f5-105">hello tools are used toomanage [shard maps](sql-database-elastic-scale-shard-map-management.md), and include hello [client library](sql-database-elastic-database-client-library.md), hello [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md), [elastic pools](sql-database-elastic-pool.md), and [queries](sql-database-elastic-query-overview.md).</span></span> 

<span data-ttu-id="a80f5-106">Ezeket a kifejezéseket használjuk [hozzáadása egy rugalmas eszközökkel shard](sql-database-elastic-scale-add-a-shard.md) és [hello RecoveryManager osztály toofix shard térkép problémák használatával](sql-database-elastic-database-recovery-manager.md).</span><span class="sxs-lookup"><span data-stu-id="a80f5-106">These terms are used in [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md).</span></span>

![Rugalmas méretezési feltételek][1]

<span data-ttu-id="a80f5-108">**Adatbázis**: az Azure SQL-adatbázist.</span><span class="sxs-lookup"><span data-stu-id="a80f5-108">**Database**: An Azure SQL database.</span></span> 

<span data-ttu-id="a80f5-109">**Adatok függő útválasztási**: hello funkció, amely lehetővé teszi egy alkalmazás tooconnect tooa shard kap egy adott horizontális kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a80f5-109">**Data dependent routing**: hello functionality that enables an application tooconnect tooa shard given a specific sharding key.</span></span> <span data-ttu-id="a80f5-110">Lásd: [adatok függő útválasztási](sql-database-elastic-scale-data-dependent-routing.md).</span><span class="sxs-lookup"><span data-stu-id="a80f5-110">See [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md).</span></span> <span data-ttu-id="a80f5-111">Hasonlítsa össze a túl**[több Shard lekérdezés](sql-database-elastic-scale-multishard-querying.md)**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-111">Compare too**[Multi-Shard Query](sql-database-elastic-scale-multishard-querying.md)**.</span></span>

<span data-ttu-id="a80f5-112">**Globális shard térkép**: hello térkép horizontális kulcsok és a megfelelő szilánkok belül között egy **shard set**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-112">**Global shard map**: hello map between sharding keys and their respective shards within a **shard set**.</span></span> <span data-ttu-id="a80f5-113">hello globális shard térkép tárolódik hello **shard térkép manager**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-113">hello global shard map is stored in hello **shard map manager**.</span></span> <span data-ttu-id="a80f5-114">Hasonlítsa össze a túl**helyi shard térkép**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-114">Compare too**local shard map**.</span></span>

<span data-ttu-id="a80f5-115">**Lista shard térkép**: shard térképre mely horizontális a kulcsok külön-külön vannak leképezve.</span><span class="sxs-lookup"><span data-stu-id="a80f5-115">**List shard map**: A shard map in which sharding keys are mapped individually.</span></span> <span data-ttu-id="a80f5-116">Hasonlítsa össze a túl**tartomány Shard térkép**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-116">Compare too**Range Shard Map**.</span></span>   

<span data-ttu-id="a80f5-117">**Helyi shard térkép**: egy shard tárolják, hello helyi shard térkép hello shardlets lévő hello shard leképezéseit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="a80f5-117">**Local shard map**: Stored on a shard, hello local shard map contains mappings for hello shardlets that reside on hello shard.</span></span>

<span data-ttu-id="a80f5-118">**Több shard lekérdezés**: hello képességét tooissue egy lekérdezést hajtanak több szilánkok; eredmények beállítása UNION ALL szemantikájú (más néven "szétterítési lekérdezés") adott vissza.</span><span class="sxs-lookup"><span data-stu-id="a80f5-118">**Multi-shard query**: hello ability tooissue a query against multiple shards; results sets are returned using UNION ALL semantics (also known as “fan-out query”).</span></span> <span data-ttu-id="a80f5-119">Hasonlítsa össze a túl**adatok függő útválasztási**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-119">Compare too**data dependent routing**.</span></span>

<span data-ttu-id="a80f5-120">**Több-bérlős** és **Single-bérlő**: Ez egy egyetlen-bérlő adatbázis és a több-bérlős adatbázis jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="a80f5-120">**Multi-tenant** and **Single-tenant**: This shows a single-tenant database and a multi-tenant database:</span></span>

![Egyetlen és több-bérlős adatbázisok](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

<span data-ttu-id="a80f5-122">Íme egy ábrázolása **szilánkos** egyetlen és több-bérlős adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="a80f5-122">Here is a representation of **sharded** single and multi-tenant databases.</span></span> 

![Egyetlen és több-bérlős adatbázisok](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

<span data-ttu-id="a80f5-124">**Tartomány shard térkép**: shard térképre mely hello a shard terjesztési stratégia több tartomány folytonos értékek alapján.</span><span class="sxs-lookup"><span data-stu-id="a80f5-124">**Range shard map**: A shard map in which hello shard distribution strategy is based on multiple ranges of contiguous values.</span></span> 

<span data-ttu-id="a80f5-125">**Táblák hivatkozhat**: táblákat, amelyek nem szilánkos, de a szilánkok replikálódnak.</span><span class="sxs-lookup"><span data-stu-id="a80f5-125">**Reference tables**: Tables that are not sharded but are replicated across shards.</span></span> <span data-ttu-id="a80f5-126">Zip-kódok például egy összefoglaló táblázatot is tárolható.</span><span class="sxs-lookup"><span data-stu-id="a80f5-126">For example, zip codes can be stored in a reference table.</span></span> 

<span data-ttu-id="a80f5-127">**A shard**: az Azure SQL-adatbázis, amely a szilánkos adatkészletet adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="a80f5-127">**Shard**: An Azure SQL database that stores data from a sharded data set.</span></span> 

<span data-ttu-id="a80f5-128">**A shard rugalmasság**: hello mindkét lehetőséget tooperform **horizontális skálázás** és **függőleges skálázás**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-128">**Shard elasticity**: hello ability tooperform both **horizontal scaling** and **vertical scaling**.</span></span>

<span data-ttu-id="a80f5-129">**Horizontálisan skálázott táblák**: táblákat, amelyek szilánkos, azaz, amelynek adatait a horizontális értékek alapján szilánkok között van elosztva.</span><span class="sxs-lookup"><span data-stu-id="a80f5-129">**Sharded tables**: Tables that are sharded, i.e., whose data is distributed across shards based on their sharding key values.</span></span> 

<span data-ttu-id="a80f5-130">**Horizontális kulcs**: határozza meg, hogy milyen adatok szilánkok pontjain oszlop értéke.</span><span class="sxs-lookup"><span data-stu-id="a80f5-130">**Sharding key**: A column value that determines how data is distributed across shards.</span></span> <span data-ttu-id="a80f5-131">hello érték típusa lehet hello a következők egyikét: **int**, **bigint**, **varbinary**, vagy **uniqueidentifier**.</span><span class="sxs-lookup"><span data-stu-id="a80f5-131">hello value type can be one of hello following: **int**, **bigint**, **varbinary**, or **uniqueidentifier**.</span></span> 

<span data-ttu-id="a80f5-132">**A shard set**: hello gyűjteménye, amelyek kiadott toohello szilánkok hello shard térkép manager ugyanazt a shard térképre.</span><span class="sxs-lookup"><span data-stu-id="a80f5-132">**Shard set**: hello collection of shards that are attributed toohello same shard map in hello shard map manager.</span></span>  

<span data-ttu-id="a80f5-133">**Shardlet**: hello adatok társított egy horizontális skálázási kulcsának egy shard egyetlen értéket.</span><span class="sxs-lookup"><span data-stu-id="a80f5-133">**Shardlet**: All of hello data associated with a single value of a sharding key on a shard.</span></span> <span data-ttu-id="a80f5-134">Egy shardlet adatátvitelt jelölik a lehető legkisebb egységére hello esetén szilánkos táblák terjesztése.</span><span class="sxs-lookup"><span data-stu-id="a80f5-134">A shardlet is hello smallest unit of data movement possible when redistributing sharded tables.</span></span> 

<span data-ttu-id="a80f5-135">**A shard térkép**: hello horizontális kulcsok és a megfelelő szilánkok közötti leképezések halmaza.</span><span class="sxs-lookup"><span data-stu-id="a80f5-135">**Shard map**: hello set of mappings between sharding keys and their respective shards.</span></span>

<span data-ttu-id="a80f5-136">**A shard térkép manager**: egy hello shard azt, a shard helyek és azon egy vagy több shard készletet tartalmazó felügyeleti-objektum és az adattárhoz.</span><span class="sxs-lookup"><span data-stu-id="a80f5-136">**Shard map manager**: A management object and data store that contains hello shard map(s), shard locations, and mappings for one or more shard sets.</span></span>

![Hozzárendelések][2]

## <a name="verbs"></a><span data-ttu-id="a80f5-138">Műveletek</span><span class="sxs-lookup"><span data-stu-id="a80f5-138">Verbs</span></span>
<span data-ttu-id="a80f5-139">**Horizontális skálázás**: hello folyamat ki (vagy a) méretezés hozzáadásával vagy eltávolításával szilánkok tooa shard térkép, a lent látható módon szilánkok gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="a80f5-139">**Horizontal scaling**: hello act of scaling out (or in) a collection of shards by adding or removing shards tooa shard map, as shown below.</span></span>

![Vízszintes és függőleges skálázás][3]

<span data-ttu-id="a80f5-141">**Egyesítési**: hello act shardlets áthelyezése két szilánkok tooone shard, és ennek megfelelően frissítése hello shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="a80f5-141">**Merge**: hello act of moving shardlets from two shards tooone shard and updating hello shard map accordingly.</span></span>

<span data-ttu-id="a80f5-142">**Shardlet áthelyezés**: hello act egy egyetlen shardlet tooa különböző shard áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="a80f5-142">**Shardlet move**: hello act of moving a single shardlet tooa different shard.</span></span> 

<span data-ttu-id="a80f5-143">**A shard**: hello act vízszintesen particionálás azonos strukturált adatok egy horizontális skálázási kulcs alapján több adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="a80f5-143">**Shard**: hello act of horizontally partitioning identically structured data across multiple databases based on a sharding key.</span></span>

<span data-ttu-id="a80f5-144">**Vegyes**: hello a folyamat több shardlets áthelyezése egy shard tooanother (általában új) szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="a80f5-144">**Split**: hello act of moving several shardlets from one shard tooanother (typically new) shard.</span></span> <span data-ttu-id="a80f5-145">A horizontális kulcs hello felhasználó által megadott módon hello vágási pont.</span><span class="sxs-lookup"><span data-stu-id="a80f5-145">A sharding key is provided by hello user as hello split point.</span></span>

<span data-ttu-id="a80f5-146">**Függőleges méretezés**: hello folyamat felfelé skálázással (vagy le) egy egyedi shard teljesítményszintjének hello.</span><span class="sxs-lookup"><span data-stu-id="a80f5-146">**Vertical Scaling**: hello act of scaling up (or down) hello performance level of an individual shard.</span></span> <span data-ttu-id="a80f5-147">Például módosítása (amelynek eredménye további számítási erőforrások) normál tooPremium a szilánkcímtárban.</span><span class="sxs-lookup"><span data-stu-id="a80f5-147">For example, changing a shard from Standard tooPremium (which results in more computing resources).</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

