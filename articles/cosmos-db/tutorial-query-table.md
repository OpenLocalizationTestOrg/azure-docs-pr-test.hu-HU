---
title: "Hogyan lehet az Azure Cosmos Adatbázisba tábla adatait? | Microsoft Docs"
description: "Ismerje meg, hogyan tábla adatait az Azure Cosmos-Adatbázisba"
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
ms.openlocfilehash: e59cfa85c6bf584e44bdc6e88cc19d67df390041
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api-preview"></a><span data-ttu-id="10906-104">Azure Cosmos DB: Hogyan tábla adatait kéri tábla API-jával (előzetes verzió)?</span><span class="sxs-lookup"><span data-stu-id="10906-104">Azure Cosmos DB: How to query table data by using the Table API (preview)?</span></span>

<span data-ttu-id="10906-105">Az Azure Cosmos DB [tábla API](table-introduction.md) (előzetes verzió) támogatja az OData és [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) Lekérdezések kulcs/érték (tábla) adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="10906-105">The Azure Cosmos DB [Table API](table-introduction.md) (preview) supports OData and [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) queries against key/value (table) data.</span></span>  

<span data-ttu-id="10906-106">Ez a cikk ismerteti a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="10906-106">This article covers the following tasks:</span></span> 

> [!div class="checklist"]
> * <span data-ttu-id="10906-107">Tábla API-val adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="10906-107">Querying data with the Table API</span></span>

<span data-ttu-id="10906-108">Ebben a cikkben a lekérdezések használja az alábbi minta `People` tábla:</span><span class="sxs-lookup"><span data-stu-id="10906-108">The queries in this article use the following sample `People` table:</span></span>

| <span data-ttu-id="10906-109">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="10906-109">PartitionKey</span></span> | <span data-ttu-id="10906-110">RowKey</span><span class="sxs-lookup"><span data-stu-id="10906-110">RowKey</span></span> | <span data-ttu-id="10906-111">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="10906-111">Email</span></span> | <span data-ttu-id="10906-112">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="10906-112">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10906-113">Grönlandi</span><span class="sxs-lookup"><span data-stu-id="10906-113">Harp</span></span> | <span data-ttu-id="10906-114">Walter</span><span class="sxs-lookup"><span data-stu-id="10906-114">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="10906-115">425-555-0101</span><span class="sxs-lookup"><span data-stu-id="10906-115">425-555-0101</span></span> |
| <span data-ttu-id="10906-116">Smith</span><span class="sxs-lookup"><span data-stu-id="10906-116">Smith</span></span> | <span data-ttu-id="10906-117">Ben</span><span class="sxs-lookup"><span data-stu-id="10906-117">Ben</span></span> | Ben@contoso.com| <span data-ttu-id="10906-118">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="10906-118">425-555-0102</span></span> |
| <span data-ttu-id="10906-119">Smith</span><span class="sxs-lookup"><span data-stu-id="10906-119">Smith</span></span> | <span data-ttu-id="10906-120">Jeff</span><span class="sxs-lookup"><span data-stu-id="10906-120">Jeff</span></span> | Jeff@contoso.com| <span data-ttu-id="10906-121">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="10906-121">425-555-0104</span></span> | 

<span data-ttu-id="10906-122">Mivel az Azure Table storage API-kkal kompatibilis Azure Cosmos DB, lásd: [lekérdezése táblákat és entitásokat] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) lekérdezés a tábla API használatával kapcsolatos részletekért.</span><span class="sxs-lookup"><span data-stu-id="10906-122">Because Azure Cosmos DB is compatible with the Azure Table storage APIs, see [Querying Tables and Entities] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) for details on how to query by using the Table API.</span></span> 

<span data-ttu-id="10906-123">Prémium szintű képességeit, amely Azure Cosmos DB nyújt további információkért lásd: [Azure Cosmos DB: tábla API](table-introduction.md) és [Develop a tábla API-t a .NET](tutorial-develop-table-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="10906-123">For more information on the premium capabilities that Azure Cosmos DB offers, see [Azure Cosmos DB: Table API](table-introduction.md) and [Develop with the Table API in .NET](tutorial-develop-table-dotnet.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="10906-124">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="10906-124">Prerequisites</span></span>

<span data-ttu-id="10906-125">A lekérdezések működjön rendelkezik Azure Cosmos DB fiókkal és a tárolóban lévő adatok entitás rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="10906-125">For these queries to work, you must have an Azure Cosmos DB account and have entity data in the container.</span></span> <span data-ttu-id="10906-126">Nem rendelkezik egyetlen, az?</span><span class="sxs-lookup"><span data-stu-id="10906-126">Don't have any of those?</span></span> <span data-ttu-id="10906-127">Fejezze be a [ötperces gyors üzembe helyezés](https://aka.ms/acdbtnetqs) vagy a [fejlesztői útmutató](https://aka.ms/acdbtabletut) hozzon létre egy fiókot, és töltse fel az adatbázist.</span><span class="sxs-lookup"><span data-stu-id="10906-127">Complete the [five-minute quickstart](https://aka.ms/acdbtnetqs) or the [developer tutorial](https://aka.ms/acdbtabletut) to create an account and populate your database.</span></span>

## <a name="query-on-partitionkey-and-rowkey"></a><span data-ttu-id="10906-128">Lekérdezés PartitionKey és RowKey</span><span class="sxs-lookup"><span data-stu-id="10906-128">Query on PartitionKey and RowKey</span></span>
<span data-ttu-id="10906-129">A PartitionKey és RowKey tulajdonság egy entitás elsődleges kulcsának alkotnak, mert a következő szintaxist segítségével azonosítja az entitást:</span><span class="sxs-lookup"><span data-stu-id="10906-129">Because the PartitionKey and RowKey properties form an entity's primary key, you can use the following special syntax to identify the entity:</span></span> 

<span data-ttu-id="10906-130">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="10906-130">**Query**</span></span>

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
<span data-ttu-id="10906-131">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="10906-131">**Results**</span></span>

| <span data-ttu-id="10906-132">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="10906-132">PartitionKey</span></span> | <span data-ttu-id="10906-133">RowKey</span><span class="sxs-lookup"><span data-stu-id="10906-133">RowKey</span></span> | <span data-ttu-id="10906-134">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="10906-134">Email</span></span> | <span data-ttu-id="10906-135">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="10906-135">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10906-136">Grönlandi</span><span class="sxs-lookup"><span data-stu-id="10906-136">Harp</span></span> | <span data-ttu-id="10906-137">Walter</span><span class="sxs-lookup"><span data-stu-id="10906-137">Walter</span></span> | Walter@contoso.com| <span data-ttu-id="10906-138">425-555-0104</span><span class="sxs-lookup"><span data-stu-id="10906-138">425-555-0104</span></span> |

<span data-ttu-id="10906-139">Másik megoldásként megadhatja ezeket a tulajdonságokat részeként a `$filter` lehetőség, mert az alábbi részben található.</span><span class="sxs-lookup"><span data-stu-id="10906-139">Alternatively, you can specify these properties as part of the `$filter` option, as shown in the following section.</span></span> <span data-ttu-id="10906-140">Vegye figyelembe, hogy a kulcstulajdonság és állandó értékek kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="10906-140">Note that the key property names and constant values are case-sensitive.</span></span> <span data-ttu-id="10906-141">A PartitionKey és a RowKey tulajdonságainak vannak karakterlánc típusú.</span><span class="sxs-lookup"><span data-stu-id="10906-141">Both the PartitionKey and RowKey properties are of type String.</span></span> 

## <a name="query-by-using-an-odata-filter"></a><span data-ttu-id="10906-142">Egy OData-szűrő segítségével lekérdezése</span><span class="sxs-lookup"><span data-stu-id="10906-142">Query by using an OData filter</span></span>
<span data-ttu-id="10906-143">Egy szűrési karakterláncot térített közben tartsa szem előtt ezeket a szabályokat:</span><span class="sxs-lookup"><span data-stu-id="10906-143">When you're constructing a filter string, keep these rules in mind:</span></span> 

* <span data-ttu-id="10906-144">Az OData protokoll specifikációja által definiált logikai operátorok segítségével összehasonlíthatja a tulajdonság értéke.</span><span class="sxs-lookup"><span data-stu-id="10906-144">Use the logical operators defined by the OData Protocol Specification to compare a property to a value.</span></span> <span data-ttu-id="10906-145">Vegye figyelembe, hogy a dinamikus érték a tulajdonság nem hasonlíthatók össze.</span><span class="sxs-lookup"><span data-stu-id="10906-145">Note that you can't compare a property to a dynamic value.</span></span> <span data-ttu-id="10906-146">A kifejezés egy oldalán állandónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="10906-146">One side of the expression must be a constant.</span></span> 
* <span data-ttu-id="10906-147">A tulajdonság nevét, a operátor és a konstans URL-kódolású szóközt kell elválasztani.</span><span class="sxs-lookup"><span data-stu-id="10906-147">The property name, operator, and constant value must be separated by URL-encoded spaces.</span></span> <span data-ttu-id="10906-148">Egy hely, mint URL-kódolású `%20`.</span><span class="sxs-lookup"><span data-stu-id="10906-148">A space is URL-encoded as `%20`.</span></span> 
* <span data-ttu-id="10906-149">Minden a szűrési karakterláncot részei kis-és nagybetűket.</span><span class="sxs-lookup"><span data-stu-id="10906-149">All parts of the filter string are case-sensitive.</span></span> 
* <span data-ttu-id="10906-150">Ugyanannál az adattípusnál a tulajdonság ahhoz, hogy a szűrő érvényes találatot, az állandó értéknek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="10906-150">The constant value must be of the same data type as the property in order for the filter to return valid results.</span></span> <span data-ttu-id="10906-151">További információ a támogatott tulajdonságtípus: [ismertetése a Table szolgáltatás adatmodell](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span><span class="sxs-lookup"><span data-stu-id="10906-151">For more information about supported property types, see [Understanding the Table Service Data Model](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model).</span></span> 

<span data-ttu-id="10906-152">Íme egy példa bemutatja, hogyan szűrése a PartitionKey és e-mailek tulajdonság egy OData lekérdezést `$filter`.</span><span class="sxs-lookup"><span data-stu-id="10906-152">Here's an example query that shows how to filter by the PartitionKey and Email properties by using an OData `$filter`.</span></span>

<span data-ttu-id="10906-153">**Lekérdezés**</span><span class="sxs-lookup"><span data-stu-id="10906-153">**Query**</span></span>

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

<span data-ttu-id="10906-154">Hogyan készítse elő különféle adatok szűrőkifejezéseket további információkért lásd: [lekérdezése táblákat és entitásokat](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span><span class="sxs-lookup"><span data-stu-id="10906-154">For more information on how to construct filter expressions for various data types, see [Querying Tables and Entities](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).</span></span>

<span data-ttu-id="10906-155">**Eredmények**</span><span class="sxs-lookup"><span data-stu-id="10906-155">**Results**</span></span>

| <span data-ttu-id="10906-156">PartitionKey</span><span class="sxs-lookup"><span data-stu-id="10906-156">PartitionKey</span></span> | <span data-ttu-id="10906-157">RowKey</span><span class="sxs-lookup"><span data-stu-id="10906-157">RowKey</span></span> | <span data-ttu-id="10906-158">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="10906-158">Email</span></span> | <span data-ttu-id="10906-159">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="10906-159">PhoneNumber</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="10906-160">Ben</span><span class="sxs-lookup"><span data-stu-id="10906-160">Ben</span></span> |<span data-ttu-id="10906-161">Smith</span><span class="sxs-lookup"><span data-stu-id="10906-161">Smith</span></span> | Ben@contoso.com| <span data-ttu-id="10906-162">425-555-0102</span><span class="sxs-lookup"><span data-stu-id="10906-162">425-555-0102</span></span> |

## <a name="query-by-using-linq"></a><span data-ttu-id="10906-163">A LINQ lekérdezés</span><span class="sxs-lookup"><span data-stu-id="10906-163">Query by using LINQ</span></span> 
<span data-ttu-id="10906-164">LINQ, amely a megfelelő OData-lekérdezési kifejezések használatával is lekérheti.</span><span class="sxs-lookup"><span data-stu-id="10906-164">You can also query by using LINQ, which translates to the corresponding OData query expressions.</span></span> <span data-ttu-id="10906-165">Íme egy példa bemutatja, hogyan hozhat létre lekérdezéseket a .NET SDK használatával:</span><span class="sxs-lookup"><span data-stu-id="10906-165">Here's an example of how to build queries by using the .NET SDK:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="10906-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10906-166">Next steps</span></span>

<span data-ttu-id="10906-167">Ebben az oktatóanyagban ezt a következők:</span><span class="sxs-lookup"><span data-stu-id="10906-167">In this tutorial, you've done the following:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="10906-168">Megtudta, hogyan lekérdezése tábla API-jával (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="10906-168">Learned how to query by using the Table API (preview)</span></span> 

<span data-ttu-id="10906-169">Most már folytathatja a következő oktatóanyag megtudhatja, miként ossza el az adatokat globális.</span><span class="sxs-lookup"><span data-stu-id="10906-169">You can now proceed to the next tutorial to learn how to distribute your data globally.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="10906-170">Az adatok globálisan terjesztése</span><span class="sxs-lookup"><span data-stu-id="10906-170">Distribute your data globally</span></span>](tutorial-global-distribution-documentdb.md)
