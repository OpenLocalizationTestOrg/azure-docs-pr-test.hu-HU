---
title: "Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel | Microsoft Docs"
description: "Az Azure Table Storage, amely egy NoSQL-adattár, a strukturált adatok felhőben való tárolásához használható."
services: cosmos-db
documentationcenter: .net
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: mimig
ms.openlocfilehash: ab77fe512d275a92da19bb5dc03da347922238a5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a><span data-ttu-id="13b52-103">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="13b52-103">Get started with Azure Table storage using .NET</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="13b52-104">Az Azure Table Storage szolgáltatás strukturált NoSQL-adatokat tárol a felhőben, így séma nélküli kulcs-/attribútumtárat biztosítva.</span><span class="sxs-lookup"><span data-stu-id="13b52-104">Azure Table storage is a service that stores structured NoSQL data in the cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="13b52-105">Mivel a Table Storage séma nélküli, az adatokat könnyen az alkalmazás változó igényeihez igazíthatja.</span><span class="sxs-lookup"><span data-stu-id="13b52-105">Because Table storage is schemaless, it's easy to adapt your data as the needs of your application evolve.</span></span> <span data-ttu-id="13b52-106">A Table Storage adataihoz számos alkalmazástípus gyorsan és költséghatékonyan férhet hozzá, a költségei pedig jellemzően alacsonyabbak, mint a hagyományos SQL hasonló mennyiségű adathoz való használata esetében.</span><span class="sxs-lookup"><span data-stu-id="13b52-106">Access to Table storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="13b52-107">A Table Storage segítségével olyan rugalmas adatkészleteket tárolhat, mint például webalkalmazások felhasználói adatai, címtárak, eszközadatok és bármilyen egyéb metaadat, amelyre a szolgáltatásnak szüksége van.</span><span class="sxs-lookup"><span data-stu-id="13b52-107">You can use Table storage to store flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="13b52-108">Egy táblán korlátlan számú entitást tárolhat, és egy tárfiók a kapacitásán belül korlátlan számú táblát tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="13b52-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up to the capacity limit of the storage account.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="13b52-109">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="13b52-109">About this tutorial</span></span>
<span data-ttu-id="13b52-110">Ez az oktatóanyag [az Azure Storage .NET-hez készült ügyféloldali kódtárának](https://www.nuget.org/packages/WindowsAzure.Storage/) használatát mutatja be néhány gyakori Azure Table Storage-forgatókönyv esetében.</span><span class="sxs-lookup"><span data-stu-id="13b52-110">This tutorial shows you how to use the [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in some common Azure Table storage scenarios.</span></span> <span data-ttu-id="13b52-111">A forgatókönyveket a táblák létrehozásának és törlésének, valamint a táblaadatok beillesztésének, frissítésének, törlésének és lekérdezésének a C# programozási nyelvben írt példáin keresztül mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="13b52-111">These scenarios are presented using C# examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="13b52-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="13b52-112">Prerequisites</span></span>

<span data-ttu-id="13b52-113">Az oktatóanyag sikeres teljesítéséhez a következőkre lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="13b52-113">You need the following to complete this tutorial successfully:</span></span>

* [<span data-ttu-id="13b52-114">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="13b52-114">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="13b52-115">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="13b52-115">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="13b52-116">Azure Configuration Manager a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="13b52-116">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [<span data-ttu-id="13b52-117">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="13b52-117">Azure storage account</span></span>](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="13b52-118">További példák</span><span class="sxs-lookup"><span data-stu-id="13b52-118">More samples</span></span>
<span data-ttu-id="13b52-119">További példák a Table Storage használatára: [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel).</span><span class="sxs-lookup"><span data-stu-id="13b52-119">For additional examples using Table storage, see [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span></span> <span data-ttu-id="13b52-120">Letöltheti és futtathatja a mintaalkalmazást, vagy megkeresheti a kódot a GitHubon.</span><span class="sxs-lookup"><span data-stu-id="13b52-120">You can download the sample application and run it, or browse the code on GitHub.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="13b52-121">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="13b52-121">Add using directives</span></span>
<span data-ttu-id="13b52-122">Adja hozzá a következő **using** irányelveket a `Program.cs` fájl elejéhez:</span><span class="sxs-lookup"><span data-stu-id="13b52-122">Add the following **using** directives to the top of the `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-the-connection-string"></a><span data-ttu-id="13b52-123">Kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="13b52-123">Parse the connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-table-service-client"></a><span data-ttu-id="13b52-124">A Table szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="13b52-124">Create the Table service client</span></span>
<span data-ttu-id="13b52-125">A [CloudTableClient][dotnet_CloudTableClient] osztály segítségével lekérheti a Table Storage-ban tárolt táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="13b52-125">The [CloudTableClient][dotnet_CloudTableClient] class enables you to retrieve tables and entities stored in Table storage.</span></span> <span data-ttu-id="13b52-126">A Table-szolgáltatásügyfél létrehozásának egyik módja a következő:</span><span class="sxs-lookup"><span data-stu-id="13b52-126">Here's one way to create the Table service client:</span></span>

```csharp
// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

<span data-ttu-id="13b52-127">Most már készen áll a Table Storage-ból adatokat olvasó és abba adatokat író kód írására.</span><span class="sxs-lookup"><span data-stu-id="13b52-127">Now you are ready to write code that reads data from and writes data to Table storage.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="13b52-128">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="13b52-128">Create a table</span></span>
<span data-ttu-id="13b52-129">A példa bemutatja, hogyan hozhat létre táblát, ha még nem rendelkezik vele:</span><span class="sxs-lookup"><span data-stu-id="13b52-129">This example shows how to create a table if it does not already exist:</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference to the table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="13b52-130">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="13b52-130">Add an entity to a table</span></span>
<span data-ttu-id="13b52-131">Az entitásokat a rendszer C#-objektumokra képezi le egy, a [TableEntity][dotnet_TableEntity] osztályból származtatott egyéni osztály használatával.</span><span class="sxs-lookup"><span data-stu-id="13b52-131">Entities map to C# objects by using a custom class derived from [TableEntity][dotnet_TableEntity].</span></span> <span data-ttu-id="13b52-132">Ha hozzá szeretne adni egy entitást egy táblához, hozzon létre egy osztályt, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="13b52-132">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="13b52-133">Az alábbi kód meghatároz egy entitásosztályt, amely az ügyfél keresztnevét használja sorkulcsnak és a vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="13b52-133">The following code defines an entity class that uses the customer's first name as the row key and last name as the partition key.</span></span> <span data-ttu-id="13b52-134">Egy adott entitás partíció- és sorkulcsa együttesen azonosítja az entitást a táblában.</span><span class="sxs-lookup"><span data-stu-id="13b52-134">Together, an entity's partition and row key uniquely identify it in the table.</span></span> <span data-ttu-id="13b52-135">Az azonos partíciókulcsú entitások gyorsabban lekérdezhetők, mint a különböző partíciókulcsúak, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb fokú méretezhetőségét teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="13b52-135">Entities with the same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="13b52-136">A táblákban tárolni kívánt entitásoknak támogatott típusúnak, például a [TableEntity][dotnet_TableEntity] osztályból származtatottnak kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="13b52-136">Entities to be stored in tables must be of a supported type, for example derived from the [TableEntity][dotnet_TableEntity] class.</span></span> <span data-ttu-id="13b52-137">A táblában tárolni kívánt entitástulajdonságoknak publikusnak kell lenniük, és támogatniuk kell az értékek beolvasását és beállítását is.</span><span class="sxs-lookup"><span data-stu-id="13b52-137">Entity properties you'd like to store in a table must be public properties of the type, and support both getting and setting of values.</span></span> <span data-ttu-id="13b52-138">Az entitástípusnak emellett elérhetővé *kell* tennie egy paraméter nélküli konstruktort is.</span><span class="sxs-lookup"><span data-stu-id="13b52-138">Also, your entity type *must* expose a parameter-less constructor.</span></span>

```csharp
public class CustomerEntity : TableEntity
{
    public CustomerEntity(string lastName, string firstName)
    {
        this.PartitionKey = lastName;
        this.RowKey = firstName;
    }

    public CustomerEntity() { }

    public string Email { get; set; }

    public string PhoneNumber { get; set; }
}
```

<span data-ttu-id="13b52-139">Az entitásokat is tartalmazó táblaműveleteket a fentebbi, Tábla létrehozása című szakaszban létrehozott [CloudTable][dotnet_CloudTable] objektum végzi el.</span><span class="sxs-lookup"><span data-stu-id="13b52-139">Table operations that involve entities are performed via the [CloudTable][dotnet_CloudTable] object that you created earlier in the "Create a table" section.</span></span> <span data-ttu-id="13b52-140">A végrehajtandó műveletet egy [TableOperation][dotnet_TableOperation] objektum képviseli.</span><span class="sxs-lookup"><span data-stu-id="13b52-140">The operation to be performed is represented by a [TableOperation][dotnet_TableOperation] object.</span></span> <span data-ttu-id="13b52-141">Az alábbi példakód bemutatja a [CloudTable][dotnet_CloudTable] objektum, majd egy **CustomerEntity** objektum létrehozását.</span><span class="sxs-lookup"><span data-stu-id="13b52-141">The following code example shows the creation of the [CloudTable][dotnet_CloudTable] object and then a **CustomerEntity** object.</span></span> <span data-ttu-id="13b52-142">A művelet előkészítéséhez létrejön egy [TableOperation][dotnet_TableOperation] objektum, amely beszúrja az ügyfélentitást a táblába.</span><span class="sxs-lookup"><span data-stu-id="13b52-142">To prepare the operation, a [TableOperation][dotnet_TableOperation] object is created to insert the customer entity into the table.</span></span> <span data-ttu-id="13b52-143">Maga a művelet a [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute] meghívásával hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="13b52-143">Finally, the operation is executed by calling [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute the insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="13b52-144">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="13b52-144">Insert a batch of entities</span></span>
<span data-ttu-id="13b52-145">Egyetlen írási művelettel egy teljes entitásköteget is beszúrhat egy táblába.</span><span class="sxs-lookup"><span data-stu-id="13b52-145">You can insert a batch of entities into a table in one write operation.</span></span> <span data-ttu-id="13b52-146">Néhány további megjegyzés a kötegműveletekkel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="13b52-146">Some other notes on batch operations:</span></span>

* <span data-ttu-id="13b52-147">Egyetlen kötegművelettel frissítéseket, törléseket és beszúrásokat hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="13b52-147">You can perform updates, deletes, and inserts in the same single batch operation.</span></span>
* <span data-ttu-id="13b52-148">Egy kötegművelet legfeljebb 100 entitást tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="13b52-148">A single batch operation can include up to 100 entities.</span></span>
* <span data-ttu-id="13b52-149">Egy adott kötegművelet összes entitásának ugyanazzal a partíciókulccsal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="13b52-149">All entities in a single batch operation must have the same partition key.</span></span>
* <span data-ttu-id="13b52-150">Ugyan lekérdezést is végre lehet hajtani kötegműveletként, de ilyenkor ez lehet a köteg egyetlen művelete.</span><span class="sxs-lookup"><span data-stu-id="13b52-150">While it is possible to perform a query as a batch operation, it must be the only operation in the batch.</span></span>

<span data-ttu-id="13b52-151">Az alábbi példakód létrehoz két entitásobjektumot, és mindkettőt hozzáadja a [TableBatchOperation][dotnet_TableBatchOperation] művelethez az [Insert][dotnet_TableBatchOperation_Insert] metódussal.</span><span class="sxs-lookup"><span data-stu-id="13b52-151">The following code example creates two entity objects and adds each to [TableBatchOperation][dotnet_TableBatchOperation] by using the [Insert][dotnet_TableBatchOperation_Insert] method.</span></span> <span data-ttu-id="13b52-152">Ezután meghívja a [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] objektumot a művelet végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="13b52-152">Then, [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] is called to execute the operation.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it to the table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it to the table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities to the batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute the batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="13b52-153">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="13b52-153">Retrieve all entities in a partition</span></span>
<span data-ttu-id="13b52-154">Ha egy táblából egy partíció összes entitását le szeretné kérdezni, használjon [TableQuery][dotnet_TableQuery] objektumot.</span><span class="sxs-lookup"><span data-stu-id="13b52-154">To query a table for all entities in a partition, use a [TableQuery][dotnet_TableQuery] object.</span></span> <span data-ttu-id="13b52-155">Az alábbi példakód megad egy szűrőt a „Smith” partíciókulcsú entitásokra.</span><span class="sxs-lookup"><span data-stu-id="13b52-155">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="13b52-156">A példa megjeleníti a konzolon a lekérdezés eredményei között szereplő entitásokhoz tartozó mezőket.</span><span class="sxs-lookup"><span data-stu-id="13b52-156">This example prints the fields of each entity in the query results to the console.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct the query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print the fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="13b52-157">Partíció entitástartományának lekérése</span><span class="sxs-lookup"><span data-stu-id="13b52-157">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="13b52-158">Ha nem szeretné az összes entitást lekérdezni egy partícióból, megadhat egy tartományt a partíciókulcs és a sorkulcs szűrőjének kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="13b52-158">If you don't want to query all entities in a partition, you can specify a range by combining the partition key filter with a row key filter.</span></span> <span data-ttu-id="13b52-159">Az alábbi példakód két szűrő segítségével kéri le az összes olyan entitást a „Smith” partícióból, melyeknél a sorkulcs (keresztnév) az ábécében az „E”-t megelőző betűvel kezdődik, majd megjeleníti a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="13b52-159">The following code example uses two filters to get all entities in partition 'Smith' where the row key (first name) starts with a letter before 'E' in the alphabet, then prints the query results.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create the table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through the results, displaying information about the entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="13b52-160">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="13b52-160">Retrieve a single entity</span></span>
<span data-ttu-id="13b52-161">Írhat egy lekérdezést egy adott entitás lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="13b52-161">You can write a query to retrieve a single, specific entity.</span></span> <span data-ttu-id="13b52-162">A következő kódban a [TableOperation][dotnet_TableOperation] művelettel adja meg a „Ben Smith” nevű ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="13b52-162">The following code uses [TableOperation][dotnet_TableOperation] to specify the customer 'Ben Smith'.</span></span> <span data-ttu-id="13b52-163">Ezzel a módszerrel a rendszer egy gyűjtemény helyett csak egyetlen entitást ad vissza, a [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] táblában visszaadott érték pedig egy **CustomerEntity** objektum lesz.</span><span class="sxs-lookup"><span data-stu-id="13b52-163">This method returns just one entity rather than a collection, and the returned value in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is a **CustomerEntity** object.</span></span> <span data-ttu-id="13b52-164">Ha egyetlen entitást szeretne lekérdezni a Table szolgáltatásból, ennek leggyorsabb módja a partíció- és sorkulcsok megadása a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="13b52-164">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the Table service.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print the phone number of the result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("The phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a><span data-ttu-id="13b52-165">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="13b52-165">Replace an entity</span></span>
<span data-ttu-id="13b52-166">Ha frissíteni kíván egy entitást, kérje le a Table szolgáltatásból, módosítsa az entitásobjektumot, majd mentse a módosításokat a Table szolgáltatásba.</span><span class="sxs-lookup"><span data-stu-id="13b52-166">To update an entity, retrieve it from the Table service, modify the entity object, and then save the changes back to the Table service.</span></span> <span data-ttu-id="13b52-167">A következő kód egy meglévő ügyfél telefonszámát módosítja.</span><span class="sxs-lookup"><span data-stu-id="13b52-167">The following code changes an existing customer's phone number.</span></span> <span data-ttu-id="13b52-168">Az [Insert][dotnet_TableOperation_Insert] parancs hívása helyett a kód a [Replace][dotnet_TableOperation_Replace] parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="13b52-168">Instead of calling [Insert][dotnet_TableOperation_Insert], this code uses [Replace][dotnet_TableOperation_Replace].</span></span> <span data-ttu-id="13b52-169">A [Replace][dotnet_TableOperation_Replace] teljesen lecseréli az entitást a kiszolgálón, hacsak az a lekérdezés óta nem módosult, mert ez esetben a művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="13b52-169">[Replace][dotnet_TableOperation_Replace] causes the entity to be fully replaced on the server, unless the entity on the server has changed since it was retrieved, in which case the operation will fail.</span></span> <span data-ttu-id="13b52-170">Erre a hibára azért van szükség, hogy az alkalmazás ne írhasson felül véletlenül egy olyan módosítást, amelyet az alkalmazás egy másik összetevője hozott létre a lekérés és a frissítés között.</span><span class="sxs-lookup"><span data-stu-id="13b52-170">This failure is to prevent your application from inadvertently overwriting a change made between the retrieval and update by another component of your application.</span></span> <span data-ttu-id="13b52-171">A hiba megfelelő kezeléséhez kérje le újra az entitást, végezze el a módosításokat (ha még érvényesek), majd hajtson végre egy újabb [Replace][dotnet_TableOperation_Replace] műveletet.</span><span class="sxs-lookup"><span data-stu-id="13b52-171">The proper handling of this failure is to retrieve the entity again, make your changes (if still valid), and then perform another [Replace][dotnet_TableOperation_Replace] operation.</span></span> <span data-ttu-id="13b52-172">A következő szakaszban megtudhatja, hogyan bírálhatja felül ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="13b52-172">The next section will show you how to override this behavior.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change the phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create the Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute the operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="13b52-173">Entitás beszúrása vagy lecserélése</span><span class="sxs-lookup"><span data-stu-id="13b52-173">Insert-or-replace an entity</span></span>
<span data-ttu-id="13b52-174">Ha az entitás a kiszolgálóról való lekérdezés óta módosult, a [Replace][dotnet_TableOperation_Replace] műveletek sikertelenek lesznek.</span><span class="sxs-lookup"><span data-stu-id="13b52-174">[Replace][dotnet_TableOperation_Replace] operations will fail if the entity has been changed since it was retrieved from the server.</span></span> <span data-ttu-id="13b52-175">Ezenkívül a [Replace][dotnet_TableOperation_Replace] művelet sikeres végrehajtásához először le kell kérnie az entitást a kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="13b52-175">Furthermore, you must retrieve the entity from the server first in order for the [Replace][dotnet_TableOperation_Replace] operation to be successful.</span></span> <span data-ttu-id="13b52-176">Néha azonban nem tudható, hogy az entitás létezik-e a kiszolgálón, és hogy a benne tárolt aktuális értékek irrelevánsak-e.</span><span class="sxs-lookup"><span data-stu-id="13b52-176">Sometimes, however, you don't know if the entity exists on the server and the current values stored in it are irrelevant.</span></span> <span data-ttu-id="13b52-177">A frissítés mindent felülír.</span><span class="sxs-lookup"><span data-stu-id="13b52-177">Your update should overwrite them all.</span></span> <span data-ttu-id="13b52-178">Ehhez használja az [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] műveletet.</span><span class="sxs-lookup"><span data-stu-id="13b52-178">To accomplish this, you would use an [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation.</span></span> <span data-ttu-id="13b52-179">Ha nem létezik az entitás, ez a művelet beszúrja, ha pedig létezik, akkor a legutóbbi frissítés idejétől függetlenül lecseréli.</span><span class="sxs-lookup"><span data-stu-id="13b52-179">This operation inserts the entity if it doesn't exist, or replaces it if it does, regardless of when the last update was made.</span></span>

<span data-ttu-id="13b52-180">Az alábbi példakód létrehoz egy ügyfélentitást Fred Joneshoz, és beilleszti azt a people táblába.</span><span class="sxs-lookup"><span data-stu-id="13b52-180">In the following code example, a customer entity for 'Fred Jones' is created and inserted into the 'people' table.</span></span> <span data-ttu-id="13b52-181">Ezután az [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] művelettel menti az azonos partíciókulccsal (Jones) és sorkulccsal (Fred) rendelkező entitást a kiszolgálón, ezúttal a PhoneNumber tulajdonság egy eltérő értékével.</span><span class="sxs-lookup"><span data-stu-id="13b52-181">Next, we use the [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation to save an entity with the same partition key (Jones) and row key (Fred) to the server, this time with a different value for the PhoneNumber property.</span></span> <span data-ttu-id="13b52-182">Mivel az [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] műveletet használjuk, minden tulajdonság értéke lecserélődik.</span><span class="sxs-lookup"><span data-stu-id="13b52-182">Because we use [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], all of its property values are replaced.</span></span> <span data-ttu-id="13b52-183">Ha azonban a táblában korábban nem szerepelt volna Fred Jones entitás, a művelet beszúrta volna.</span><span class="sxs-lookup"><span data-stu-id="13b52-183">However, if a 'Fred Jones' entity hadn't already existed in the table, it would have been inserted.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable object that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create the TableOperation object that inserts the customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute the operation.
table.Execute(insertOperation);

// Create another customer entity with the same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it to the
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create the InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute the operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, the entity would be
// added to the table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="13b52-184">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="13b52-184">Query a subset of entity properties</span></span>
<span data-ttu-id="13b52-185">Egy táblalekérdezéssel egy entitás bizonyos tulajdonságait is lekérdezheti az összes helyett.</span><span class="sxs-lookup"><span data-stu-id="13b52-185">A table query can retrieve just a few properties from an entity instead of all the entity properties.</span></span> <span data-ttu-id="13b52-186">Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében.</span><span class="sxs-lookup"><span data-stu-id="13b52-186">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="13b52-187">Az alábbi kódban szereplő lekérdezés csak a táblában található entitásokhoz tartozó e-mail-címeket kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="13b52-187">The query in the following code returns only the email addresses of entities in the table.</span></span> <span data-ttu-id="13b52-188">Ez a [DynamicTableEntity][dotnet_DynamicTableEntity] és az [EntityResolver][dotnet_EntityResolver] lekérdezésekkel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="13b52-188">This is done by using a query of [DynamicTableEntity][dotnet_DynamicTableEntity] and also [EntityResolver][dotnet_EntityResolver].</span></span> <span data-ttu-id="13b52-189">A leképezésről az [Introducing Upsert and Query Projection][blog_post_upsert] (Az upsert művelet és a lekérdezésleképezés bemutatása) című blogbejegyzésből tudhat meg többet.</span><span class="sxs-lookup"><span data-stu-id="13b52-189">You can learn more about projection in the [Introducing Upsert and Query Projection blog post][blog_post_upsert].</span></span> <span data-ttu-id="13b52-190">A Storage Emulator nem támogatja a leképezést, így a kód csak a Table szolgáltatásbeli fiókkal működik.</span><span class="sxs-lookup"><span data-stu-id="13b52-190">Projection is not supported by the storage emulator, so this code runs only when you're using an account in the Table service.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define the query, and select only the Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver to work with the entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a><span data-ttu-id="13b52-191">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="13b52-191">Delete an entity</span></span>
<span data-ttu-id="13b52-192">A lekérdezés után egyszerűen törölheti az entitásokat az entitások frissítésénél bemutatott minta alapján.</span><span class="sxs-lookup"><span data-stu-id="13b52-192">You can easily delete an entity after you have retrieved it by using the same pattern shown for updating an entity.</span></span> <span data-ttu-id="13b52-193">Az alábbi kód lekérdez, majd töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="13b52-193">The following code retrieves and deletes a customer entity.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign the result to a CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create the Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute the operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve the entity.");
}
```

## <a name="delete-a-table"></a><span data-ttu-id="13b52-194">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="13b52-194">Delete a table</span></span>
<span data-ttu-id="13b52-195">Végezetül pedig az alábbi példakóddal törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="13b52-195">Finally, the following code example deletes a table from a storage account.</span></span> <span data-ttu-id="13b52-196">A törölt táblák a törlés után egy ideig nem hozhatók létre újra.</span><span class="sxs-lookup"><span data-stu-id="13b52-196">A table that has been deleted will be unavailable to be re-created for a period of time following the deletion.</span></span>

```csharp
// Retrieve the storage account from the connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create the table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create the CloudTable that represents the "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete the table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a><span data-ttu-id="13b52-197">Oldalak entitásainak aszinkron lekérése</span><span class="sxs-lookup"><span data-stu-id="13b52-197">Retrieve entities in pages asynchronously</span></span>
<span data-ttu-id="13b52-198">Ha sok entitást olvas, és az összes visszaadott entitás helyett csak az éppen lekérdezett entitásokat szeretné feldolgozni/megjeleníteni, szegmentált lekérdezéssel kérje le az entitásokat.</span><span class="sxs-lookup"><span data-stu-id="13b52-198">If you are reading a large number of entities, and you want to process/display entities as they are retrieved rather than waiting for them all to return, you can retrieve entities by using a segmented query.</span></span> <span data-ttu-id="13b52-199">A példa bemutatja, hogy az Async-Await mintázattal hogyan kérhetők le eredmények az oldalakról úgy, hogy ne legyen letiltva a végrehajtás, amíg egy nagy eredménykészletre várakozik.</span><span class="sxs-lookup"><span data-stu-id="13b52-199">This example shows how to return results in pages by using the Async-Await pattern so that execution is not blocked while you're waiting for a large set of results to return.</span></span> <span data-ttu-id="13b52-200">További információk az Async-Await mintázat használatáról .NET-keretrendszerben: [Asynchronous Programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx) (Aszinkron programozás az Async és Await műveletekkel (C# és Visual Basic)).</span><span class="sxs-lookup"><span data-stu-id="13b52-200">For more details on using the Async-Await pattern in .NET, see [Asynchronous programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span></span>

```csharp
// Initialize a default TableQuery to retrieve all the entities in the table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize the continuation token to null to start from the beginning of the table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up to 1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign the new continuation token to tell the service where to
    // continue on the next iteration (or null if it has reached the end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print the number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating the end of the table.
} while(continuationToken != null);
```

## <a name="next-steps"></a><span data-ttu-id="13b52-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="13b52-201">Next steps</span></span>
<span data-ttu-id="13b52-202">Most, hogy mér megismerte a Table Storage alapjait, az alábbi hivatkozásokból tájékozódhat az összetettebb tárolási feladatok elvégzéséről is:</span><span class="sxs-lookup"><span data-stu-id="13b52-202">Now that you've learned the basics of Table storage, follow these links to learn about more complex storage tasks:</span></span>

* <span data-ttu-id="13b52-203">A [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="13b52-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="13b52-204">További Table Storage-példákat a [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel) című cikkben tekinthet meg.</span><span class="sxs-lookup"><span data-stu-id="13b52-204">See more Table storage samples in [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span></span>
* <span data-ttu-id="13b52-205">A Table Service elérhető API-kat részletesen ismertető referenciadokumentációjának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="13b52-205">View the Table service reference documentation for complete details about available APIs:</span></span>
* [<span data-ttu-id="13b52-206">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="13b52-206">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [<span data-ttu-id="13b52-207">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="13b52-207">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="13b52-208">Az [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md) használatával megtudhatja, hogyan egyszerűsítheti az Azure Storage használatához írt kódot</span><span class="sxs-lookup"><span data-stu-id="13b52-208">Learn how to simplify the code you write to work with Azure Storage by using the [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span></span>
* <span data-ttu-id="13b52-209">Az Azure-ban való adattárolás további lehetőségeiről tekintse meg a többi szolgáltatás-útmutatót.</span><span class="sxs-lookup"><span data-stu-id="13b52-209">View more feature guides to learn about additional options for storing data in Azure.</span></span>
* <span data-ttu-id="13b52-210">[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) (Az Azure Blob Storage használatának első lépései a .NET-keretrendszerrel) a strukturálatlan adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="13b52-210">[Get started with Azure Blob storage using .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md) to store unstructured data.</span></span>
* <span data-ttu-id="13b52-211">[Csatlakozzon az SQL Database adatbázishoz .NET (C#) használatával](../sql-database/sql-database-develop-dotnet-simple.md) a relációs adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="13b52-211">[Connect to SQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) to store relational data.</span></span>

[Download and install the Azure SDK for .NET]: /develop/net/
[Creating an Azure Project in Visual Studio]: http://msdn.microsoft.com/library/azure/ee405487.aspx

[blog_post_upsert]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx

[dotnet_api_ref]: https://msdn.microsoft.com/library/azure/mt347887.aspx
[dotnet_CloudTableClient]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtableclient.aspx
[dotnet_CloudTable]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.aspx
[dotnet_CloudTable_Execute]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.execute.aspx
[dotnet_CloudTable_ExecuteBatch]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.cloudtable.executebatch.aspx
[dotnet_DynamicTableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.dynamictableentity.aspx
[dotnet_EntityResolver]: https://msdn.microsoft.com/library/jj733144.aspx
[dotnet_TableBatchOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.aspx
[dotnet_TableBatchOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablebatchoperation.insert.aspx
[dotnet_TableEntity]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableentity.aspx
[dotnet_TableOperation]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.aspx
[dotnet_TableOperation_Insert]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insert.aspx
[dotnet_TableOperation_InsertOrReplace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.insertorreplace.aspx
[dotnet_TableOperation_Replace]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableoperation.replace.aspx
[dotnet_TableQuery]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tablequery.aspx
[dotnet_TableResult]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.aspx
[dotnet_TableResult_Result]: https://msdn.microsoft.com/library/microsoft.windowsazure.storage.table.tableresult.result.aspx
