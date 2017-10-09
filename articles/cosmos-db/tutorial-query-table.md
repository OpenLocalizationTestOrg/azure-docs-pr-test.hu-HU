---
title: "aaaHow tooquery tábla adatokat az Adatbázisba az Azure Cosmos? | Microsoft Docs"
description: "Ismerje meg az Azure Cosmos Adatbázisba tooquery adatait"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 05/10/2017
ms.author: govindk
ms.openlocfilehash: 32526c3488c589c5be3a4a2f174aa769570f0c0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-how-tooquery-table-data-by-using-hello-table-api-preview"></a><span data-ttu-id="1b6a8-104">A Cosmos DB Azure: Hogyan tooquery tábla adatai használatával hello tábla API (előzetes verzió)?</span><span class="sxs-lookup"><span data-stu-id="1b6a8-104">Azure Cosmos DB: How tooquery table data by using hello Table API (preview)?</span></span>

<span data-ttu-id="1b6a8-105">hello Azure Cosmos DB [tábla API](table-introduction.md) (előzetes verzió) támogatja az OData és [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) Lekérdezések kulcs/érték (tábla) adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-105">hello Azure Cosmos DB [Table API](table-introduction.md) (preview) supports OData and [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) queries against key/value (table) data.</span></span>  

<span data-ttu-id="1b6a8-106">Ez a cikk ismerteti a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-106">This article covers hello following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="1b6a8-107">Tábla API hello az adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1b6a8-107">Querying data with hello Table API</span></span>

<span data-ttu-id="1b6a8-108">hello ebben a cikkben lekérdezések használja a következő minta hello `People` tábla:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-108">hello queries in this article use hello following sample `People` table:</span></span>

| <span data-ttu-id="1b6a8-109">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-109">PartitionKey</span></span> | <span data-ttu-id="1b6a8-110">RowKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-110">RowKey</span></span> | <span data-ttu-id="1b6a8-111">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="1b6a8-111">Email</span></span> | <span data-ttu-id="1b6a8-112">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="1b6a8-112">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1b6a8-113">Grönlandi</span><span class="sxs-lookup"><span data-stu-id="1b6a8-113">Harp</span></span> | <span data-ttu-id="1b6a8-114">Walter</span><span class="sxs-lookup"><span data-stu-id="1b6a8-114">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="1b6a8-115">425-555-0101</span><span class="sxs-lookup"><span data-stu-id="1b6a8-115">425-555-0101</span></span> |
| <span data-ttu-id="1b6a8-116">Smith</span><span class="sxs-lookup"><span data-stu-id="1b6a8-116">Smith</span></span> | <span data-ttu-id="1b6a8-117">Ben</span><span class="sxs-lookup"><span data-stu-id="1b6a8-117">Ben</span></span> | Ben@contoso.com| <span data-ttu-id="1b6a8-118">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="1b6a8-118">425-555-0102</span></span> |
| <span data-ttu-id="1b6a8-119">Smith</span><span class="sxs-lookup"><span data-stu-id="1b6a8-119">Smith</span></span> | <span data-ttu-id="1b6a8-120">Jeff</span><span class="sxs-lookup"><span data-stu-id="1b6a8-120">Jeff</span></span> | Jeff@contoso.com| <span data-ttu-id="1b6a8-121">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="1b6a8-121">425-555-0104</span></span> | 

<span data-ttu-id="1b6a8-122">Mivel Azure Cosmos DB hello Azure Table storage API-kkal kompatibilis, lásd: [lekérdezése táblákat és entitásokat] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) talál részletes információt hogyan használatával tooquery hello Tábla API.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-122">Because Azure Cosmos DB is compatible with hello Azure Table storage APIs, see [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) for details on how tooquery by using hello Table API.</span></span> 

<span data-ttu-id="1b6a8-123">Hello prémium képességeket kínál, amelyek Azure Cosmos DB nyújt további információkért lásd: [Azure Cosmos DB: tábla API](table-introduction.md) és [Develop a .NET-tábla API hello](tutorial-develop-table-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="1b6a8-123">For more information on hello premium capabilities that Azure Cosmos DB offers, see [Azure Cosmos DB: Table API](table-introduction.md) and [Develop with hello Table API in .NET](tutorial-develop-table-dotnet.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="1b6a8-124">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1b6a8-124">Prerequisites</span></span>

<span data-ttu-id="1b6a8-125">Az alábbi lekérdezéseket toowork rendelkezik Azure Cosmos DB fiókkal és entitás adatokat hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-125">For these queries toowork, you must have an Azure Cosmos DB account and have entity data in hello container.</span></span> <span data-ttu-id="1b6a8-126">Nem rendelkezik egyetlen, az?</span><span class="sxs-lookup"><span data-stu-id="1b6a8-126">Don't have any of those?</span></span> <span data-ttu-id="1b6a8-127">Teljes hello [ötperces gyors üzembe helyezés](https://aka.ms/acdbtnetqs) vagy hello [fejlesztői útmutató](https://aka.ms/acdbtabletut) toocreate egy fiókot, és töltse fel az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-127">Complete hello [five-minute quickstart](https://aka.ms/acdbtnetqs) or hello [developer tutorial](https://aka.ms/acdbtabletut) toocreate an account and populate your database.</span></span>

## <a name="query-on-partitionkey-and-rowkey"></a><span data-ttu-id="1b6a8-128">Lekérdezés PartitionKey és RowKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-128">Query on PartitionKey and RowKey</span></span>
<span data-ttu-id="1b6a8-129">Hello PartitionKey és RowKey tulajdonság egy entitás elsődleges kulcsának alkotnak, mert a következő szintaxist tooidentify hello entitás hello is használhatja:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-129">Because hello PartitionKey and RowKey properties form an entity's primary key, you can use hello following special syntax tooidentify hello entity:</span></span> 

<span data-ttu-id="1b6a8-130">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="1b6a8-130">**Query**</span></span>

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
<span data-ttu-id="1b6a8-131">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="1b6a8-131">**Results**</span></span>

| <span data-ttu-id="1b6a8-132">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-132">PartitionKey</span></span> | <span data-ttu-id="1b6a8-133">RowKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-133">RowKey</span></span> | <span data-ttu-id="1b6a8-134">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="1b6a8-134">Email</span></span> | <span data-ttu-id="1b6a8-135">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="1b6a8-135">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1b6a8-136">Grönlandi</span><span class="sxs-lookup"><span data-stu-id="1b6a8-136">Harp</span></span> | <span data-ttu-id="1b6a8-137">Walter</span><span class="sxs-lookup"><span data-stu-id="1b6a8-137">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="1b6a8-138">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="1b6a8-138">425-555-0104</span></span> |

<span data-ttu-id="1b6a8-139">Másik megoldásként megadhatja ezeket a tulajdonságokat hello részeként `$filter` lehetőség használata, mert a következő szakasz hello látható.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-139">Alternatively, you can specify these properties as part of hello `$filter` option, as shown in hello following section.</span></span> <span data-ttu-id="1b6a8-140">Vegye figyelembe, hogy hello kulcstulajdonság és állandó értékek kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-140">Note that hello key property names and constant values are case-sensitive.</span></span> <span data-ttu-id="1b6a8-141">Hello PartitionKey és a RowKey tulajdonságainak vannak karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-141">Both hello PartitionKey and RowKey properties are of type String.</span></span> 

## <a name="query-by-using-an-odata-filter"></a><span data-ttu-id="1b6a8-142">Egy OData-szűrő segítségével lekérdezése</span><span class="sxs-lookup"><span data-stu-id="1b6a8-142">Query by using an OData filter</span></span>
<span data-ttu-id="1b6a8-143">Egy szűrési karakterláncot térített közben tartsa szem előtt ezeket a szabályokat:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-143">When you're constructing a filter string, keep these rules in mind:</span></span> 

* <span data-ttu-id="1b6a8-144">Hello hello OData protokoll specifikációja toocompare tooa tulajdonság értéke által meghatározott logikai operátorok használatára.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-144">Use hello logical operators defined by hello OData Protocol Specification toocompare a property tooa value.</span></span> <span data-ttu-id="1b6a8-145">Vegye figyelembe, hogy tooa dinamikus tulajdonság értéke nem hasonlíthatók össze.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-145">Note that you can't compare a property tooa dynamic value.</span></span> <span data-ttu-id="1b6a8-146">Hello kifejezés oldalán állandónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-146">One side of hello expression must be a constant.</span></span> 
* <span data-ttu-id="1b6a8-147">hello tulajdonság nevét, a operátor és a konstans URL-kódolású szóközt kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-147">hello property name, operator, and constant value must be separated by URL-encoded spaces.</span></span> <span data-ttu-id="1b6a8-148">Egy hely, mint URL-kódolású `%20`.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-148">A space is URL-encoded as `%20`.</span></span> 
* <span data-ttu-id="1b6a8-149">Minden hello szűrési karakterláncot részei kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-149">All parts of hello filter string are case-sensitive.</span></span> 
* <span data-ttu-id="1b6a8-150">hello hello állandó értéknek kell lennie ahhoz, hogy hello szűrő tooreturn érvényes eredmények hello tulajdonság írja ugyanazokat az adatokat.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-150">hello constant value must be of hello same data type as hello property in order for hello filter tooreturn valid results.</span></span> <span data-ttu-id="1b6a8-151">További információ a támogatott tulajdonságtípus: [ismertetése hello tábla szolgáltatás adatmodell](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span><span class="sxs-lookup"><span data-stu-id="1b6a8-151">For more information about supported property types, see [Understanding hello Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span></span> 

<span data-ttu-id="1b6a8-152">Íme egy példa bemutatja, hogyan által toofilter hello PartitionKey lekérdezést és E-mail-tulajdonságok használatával egy OData `$filter`.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-152">Here's an example query that shows how toofilter by hello PartitionKey and Email properties by using an OData `$filter`.</span></span>

<span data-ttu-id="1b6a8-153">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="1b6a8-153">**Query**</span></span>

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

<span data-ttu-id="1b6a8-154">Hogyan tooconstruct szűrőkifejezés különböző adattípusok a további információkért lásd: [lekérdezése táblákat és entitásokat](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span><span class="sxs-lookup"><span data-stu-id="1b6a8-154">For more information on how tooconstruct filter expressions for various data types, see [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span></span>

<span data-ttu-id="1b6a8-155">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="1b6a8-155">**Results**</span></span>

| <span data-ttu-id="1b6a8-156">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-156">PartitionKey</span></span> | <span data-ttu-id="1b6a8-157">RowKey</span><span class="sxs-lookup"><span data-stu-id="1b6a8-157">RowKey</span></span> | <span data-ttu-id="1b6a8-158">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="1b6a8-158">Email</span></span> | <span data-ttu-id="1b6a8-159">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="1b6a8-159">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="1b6a8-160">Ben</span><span class="sxs-lookup"><span data-stu-id="1b6a8-160">Ben</span></span> |<span data-ttu-id="1b6a8-161">Smith</span><span class="sxs-lookup"><span data-stu-id="1b6a8-161">Smith</span></span> | Ben@contoso.com| <span data-ttu-id="1b6a8-162">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="1b6a8-162">425-555-0102</span></span> |

## <a name="query-by-using-linq"></a><span data-ttu-id="1b6a8-163">A LINQ lekérdezés</span><span class="sxs-lookup"><span data-stu-id="1b6a8-163">Query by using LINQ</span></span> 
<span data-ttu-id="1b6a8-164">LINQ, amely toohello megfelelő OData-lekérdezési kifejezések használatával is lekérheti.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-164">You can also query by using LINQ, which translates toohello corresponding OData query expressions.</span></span> <span data-ttu-id="1b6a8-165">Íme egy példa hogyan toobuild lekérdezések használatával hello .NET SDK-val:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-165">Here's an example of how toobuild queries by using hello .NET SDK:</span></span>

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a><span data-ttu-id="1b6a8-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1b6a8-166">Next steps</span></span>

<span data-ttu-id="1b6a8-167">Ebben az oktatóanyagban hello következő régebben már kötöttek:</span><span class="sxs-lookup"><span data-stu-id="1b6a8-167">In this tutorial, you've done hello following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b6a8-168">Megtudta, hogyan használatával tooquery hello tábla API (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="1b6a8-168">Learned how tooquery by using hello Table API (preview)</span></span> 

<span data-ttu-id="1b6a8-169">Most már folytathatja toohello következő útmutató toolearn hogyan toodistribute az adatok globálisan.</span><span class="sxs-lookup"><span data-stu-id="1b6a8-169">You can now proceed toohello next tutorial toolearn how toodistribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1b6a8-170">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="1b6a8-170">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)
