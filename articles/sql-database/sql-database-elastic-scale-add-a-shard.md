---
title: "A rugalmas adatbázis-eszközökkel szilánkcímtárban hozzáadása |} Microsoft Docs"
description: "Állítsa be a rugalmas, méretezhető API-k használata új szilánkok hozzáadása a szilánkcímtárban."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 6a91ea2251ea3b748faba5c97765bfded9c00234
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a><span data-ttu-id="28f45-103">A rugalmas adatbázis-eszközökkel szilánkcímtárban hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28f45-103">Adding a shard using Elastic Database tools</span></span>
## <a name="to-add-a-shard-for-a-new-range-or-key"></a><span data-ttu-id="28f45-104">A szilánkok egy új tartományt vagy a kulcs hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28f45-104">To add a shard for a new range or key</span></span>
<span data-ttu-id="28f45-105">Alkalmazások egyszerűen adja hozzá az új kulcsokat vagy egy már létező shard térkép kulcstartományokkal várt adatok kezelésének új szilánkok gyakran kell.</span><span class="sxs-lookup"><span data-stu-id="28f45-105">Applications often need to simply add new shards to handle data that is expected from new keys or key ranges, for a shard map that already exists.</span></span> <span data-ttu-id="28f45-106">Például egy bérlő-azonosító szerint szilánkos alkalmazás egy új shard létrehozni egy új bérlőt kell, vagy adatok szilánkos havi esetleg egy új shard kiépített új havonta megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="28f45-106">For example, an application sharded by Tenant ID may need to provision a new shard for a new tenant, or data sharded monthly may need a new shard provisioned before the start of each new month.</span></span> 

<span data-ttu-id="28f45-107">Az új kulcs értékek tartományán már nem része egy meglévő leképezést, akkor adja hozzá az új shard, és rendelje hozzá az új kulcs vagy az és, hogy a shard nagyon egyszerű.</span><span class="sxs-lookup"><span data-stu-id="28f45-107">If the new range of key values is not already part of an existing mapping, it is very simple to add the new shard and associate the new key or range to that shard.</span></span> 

### <a name="example--adding-a-shard-and-its-range-to-an-existing-shard-map"></a><span data-ttu-id="28f45-108">Példa: egy shard és a tartomány hozzáadása meglévő shard térkép</span><span class="sxs-lookup"><span data-stu-id="28f45-108">Example:  adding a shard and its range to an existing shard map</span></span>
<span data-ttu-id="28f45-109">Ebben a példában a [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) a [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) módszerek, és létrehoz egy példányát a [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.)osztály.</span><span class="sxs-lookup"><span data-stu-id="28f45-109">This sample uses the [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) the [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) methods, and creates an instance of the [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) class.</span></span> <span data-ttu-id="28f45-110">Néven adatbázis az alábbi minta **sample_shard_2** , minden szükséges sémaobjektumok saját létrejöttek ahhoz, hogy a tartomány [300, 400).</span><span class="sxs-lookup"><span data-stu-id="28f45-110">In the sample below, a database named **sample_shard_2** and all necessary schema objects inside of it have been created to hold range [300, 400).</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create the mapping and associate it with the new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


<span data-ttu-id="28f45-111">Alternatív megoldásként a Powershell segítségével hozzon létre egy új Shard térkép Manager.</span><span class="sxs-lookup"><span data-stu-id="28f45-111">As an alternative, you can use Powershell to create a new Shard Map Manager.</span></span> <span data-ttu-id="28f45-112">Példa: elérhető [Itt](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span><span class="sxs-lookup"><span data-stu-id="28f45-112">An example is available [here](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).</span></span>

## <a name="to-add-a-shard-for-an-empty-part-of-an-existing-range"></a><span data-ttu-id="28f45-113">Egy meglévő tartomány üres részét képezik a szilánkcímtárban hozzáadása</span><span class="sxs-lookup"><span data-stu-id="28f45-113">To add a shard for an empty part of an existing range</span></span>
<span data-ttu-id="28f45-114">Bizonyos körülmények között előfordulhat, hogy már van leképezve a shard széles és részben megtelt adatokkal, de az kívánja most átirányítja egy másik shard jövőbeli adatokat.</span><span class="sxs-lookup"><span data-stu-id="28f45-114">In some circumstances, you may have already mapped a range to a shard and partially filled it with data, but you now want upcoming data to be directed to a different shard.</span></span> <span data-ttu-id="28f45-115">Például, hogy a shard naptartomány által, és már lefoglalt 50 nap kell egy shard, de 24 napon szeretné megnyílik egy másik shard a jövőbeli adatokat.</span><span class="sxs-lookup"><span data-stu-id="28f45-115">For example, you shard by day range and have already allocated 50 days to a shard, but on day 24, you want future data to land in a different shard.</span></span> <span data-ttu-id="28f45-116">A rugalmas adatbázis [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md) hajthatják végre a műveletet, de ha az adatmozgatás nem szükséges (például a tartományhoz [25, 50 napnyi adat), azaz, napi 25 kizárólagos, 50 között lehet még nem létezik) Ez végezheti el a szilánkok térkép felügyeleti API-k teljes mértékben közvetlenül használatával.</span><span class="sxs-lookup"><span data-stu-id="28f45-116">The elastic database [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) can perform this operation, but if data movement is not necessary (for example, data for the range of days [25, 50), i.e., day 25 inclusive to 50 exclusive, does not yet exist) you can perform this entirely using the Shard Map Management APIs directly.</span></span>

### <a name="example-splitting-a-range-and-assigning-the-empty-portion-to-a-newly-added-shard"></a><span data-ttu-id="28f45-117">Példa: egy tartományt a felosztás, és az üres rész hozzárendelése egy újonnan hozzáadott shard</span><span class="sxs-lookup"><span data-stu-id="28f45-117">Example: splitting a range and assigning the empty portion to a newly-added shard</span></span>
<span data-ttu-id="28f45-118">Létrehozott egy "sample_shard_2" és az összes szükséges séma objektum saját nevű adatbázis.</span><span class="sxs-lookup"><span data-stu-id="28f45-118">A database named “sample_shard_2” and all necessary schema objects inside of it have been created.</span></span>  

    // sm is a RangeShardMap object.
    // Add a new shard to hold the range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split the Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) to different shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline to a different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

<span data-ttu-id="28f45-119">**Fontos**: Ezzel a technikával használja, csak ha biztos abban, hogy a tartományon a frissített leképezése nem üres.</span><span class="sxs-lookup"><span data-stu-id="28f45-119">**Important**:  Use this technique only if you are certain that the range for the updated mapping is empty.</span></span>  <span data-ttu-id="28f45-120">A fenti módszerek nem ellenőrzi adatokat át, a tartományhoz, így legjobb ellenőrzések szerepeljenek a kódot.</span><span class="sxs-lookup"><span data-stu-id="28f45-120">The methods above do not check data for the range being moved, so it is best to include checks in your code.</span></span>  <span data-ttu-id="28f45-121">Ha a sorok léteznek éppen áthelyezik a tartományban, a tényleges adatok terjesztési nem fog egyezni a frissített shard leképezés.</span><span class="sxs-lookup"><span data-stu-id="28f45-121">If rows exist in the range being moved, the actual data distribution will not match the updated shard map.</span></span> <span data-ttu-id="28f45-122">Használja a [vegyes egyesítéses eszköz](sql-database-elastic-scale-overview-split-and-merge.md) ezekben az esetekben inkább a művelet végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="28f45-122">Use the [split-merge tool](sql-database-elastic-scale-overview-split-and-merge.md) to perform the operation instead in these cases.</span></span>  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

