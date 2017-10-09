---
title: "aaaGet Azure Table storage használatának .NET használatába |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: storage
documentationcenter: .net
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: fe46d883-7bed-49dd-980e-5c71df36adb3
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 04/10/2017
ms.author: marsma
ms.openlocfilehash: 9635079d61d874ff7f4bc9e7d610e0ad54b4fd6f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a><span data-ttu-id="c76fb-103">Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="c76fb-103">Get started with Azure Table storage using .NET</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

<span data-ttu-id="c76fb-104">Azure Table storage egy olyan szolgáltatás, amely strukturált NoSQL hello felhőben biztosít egy kulcsattribútum adattároló séma nélküli kialakítás tárolja.</span><span class="sxs-lookup"><span data-stu-id="c76fb-104">Azure Table storage is a service that stores structured NoSQL data in hello cloud, providing a key/attribute store with a schemaless design.</span></span> <span data-ttu-id="c76fb-105">Mivel a Table storage séma nélküli, akkor egyszerűen tooadapt alkalmazás igényeinek változásával kell hello az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c76fb-105">Because Table storage is schemaless, it's easy tooadapt your data as hello needs of your application evolve.</span></span> <span data-ttu-id="c76fb-106">Hozzáférés tooTable tárolási adatok gyors és költséghatékony, számos különböző típusú alkalmazásokat, és általában kevesebb költséggel jár, mint a hagyományos SQL hasonló adatmennyiséggel.</span><span class="sxs-lookup"><span data-stu-id="c76fb-106">Access tooTable storage data is fast and cost-effective for many types of applications, and is typically lower in cost than traditional SQL for similar volumes of data.</span></span>

<span data-ttu-id="c76fb-107">Webes alkalmazásokhoz, címtárakat, eszközadatokat és egyéb metaadatokat a szolgáltatásnak szüksége van a Table storage toostore rugalmas adatkészleteket például a felhasználói adatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="c76fb-107">You can use Table storage toostore flexible datasets like user data for web applications, address books, device information, or other types of metadata your service requires.</span></span> <span data-ttu-id="c76fb-108">Korlátlan számú entitást tárolhat egy táblát, és a storage-fiókok tartalmazhat korlátlan számú táblát toohello kapacitásán belül hello storage-fiók mentése.</span><span class="sxs-lookup"><span data-stu-id="c76fb-108">You can store any number of entities in a table, and a storage account may contain any number of tables, up toohello capacity limit of hello storage account.</span></span>

### <a name="about-this-tutorial"></a><span data-ttu-id="c76fb-109">Az oktatóanyag ismertetése</span><span class="sxs-lookup"><span data-stu-id="c76fb-109">About this tutorial</span></span>
<span data-ttu-id="c76fb-110">Az oktatóanyag bemutatja, hogyan toouse hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) az egyes Azure Table storage szabhatják.</span><span class="sxs-lookup"><span data-stu-id="c76fb-110">This tutorial shows you how toouse hello [Azure Storage Client Library for .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) in some common Azure Table storage scenarios.</span></span> <span data-ttu-id="c76fb-111">A forgatókönyveket a táblák létrehozásának és törlésének, valamint a táblaadatok beillesztésének, frissítésének, törlésének és lekérdezésének a C# programozási nyelvben írt példáin keresztül mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="c76fb-111">These scenarios are presented using C# examples for creating and deleting a table, and inserting, updating, deleting, and querying table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c76fb-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c76fb-112">Prerequisites</span></span>

<span data-ttu-id="c76fb-113">Meg kell hello toocomplete követően ez az oktatóanyag sikeresen:</span><span class="sxs-lookup"><span data-stu-id="c76fb-113">You need hello following toocomplete this tutorial successfully:</span></span>

* [<span data-ttu-id="c76fb-114">Microsoft Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c76fb-114">Microsoft Visual Studio</span></span>](https://www.visualstudio.com/downloads/)
* [<span data-ttu-id="c76fb-115">Az Azure Storage .NET-hez készült ügyféloldali kódtára</span><span class="sxs-lookup"><span data-stu-id="c76fb-115">Azure Storage Client Library for .NET</span></span>](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [<span data-ttu-id="c76fb-116">Azure Configuration Manager a .NET-hez</span><span class="sxs-lookup"><span data-stu-id="c76fb-116">Azure Configuration Manager for .NET</span></span>](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [<span data-ttu-id="c76fb-117">Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="c76fb-117">Azure storage account</span></span>](storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a><span data-ttu-id="c76fb-118">További példák</span><span class="sxs-lookup"><span data-stu-id="c76fb-118">More samples</span></span>
<span data-ttu-id="c76fb-119">További példák a Table Storage használatára: [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel).</span><span class="sxs-lookup"><span data-stu-id="c76fb-119">For additional examples using Table storage, see [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/).</span></span> <span data-ttu-id="c76fb-120">Töltse le a mintaalkalmazást hello és futtatáshoz, vagy keresse meg a hello kódja a Githubon.</span><span class="sxs-lookup"><span data-stu-id="c76fb-120">You can download hello sample application and run it, or browse hello code on GitHub.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a><span data-ttu-id="c76fb-121">Hozzáadás irányelvekkel</span><span class="sxs-lookup"><span data-stu-id="c76fb-121">Add using directives</span></span>
<span data-ttu-id="c76fb-122">Adja hozzá a következő hello **használatával** irányelvek toohello felső részén hello `Program.cs` fájlt:</span><span class="sxs-lookup"><span data-stu-id="c76fb-122">Add hello following **using** directives toohello top of hello `Program.cs` file:</span></span>

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a><span data-ttu-id="c76fb-123">Hello kapcsolati karakterlánc elemzése</span><span class="sxs-lookup"><span data-stu-id="c76fb-123">Parse hello connection string</span></span>
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a><span data-ttu-id="c76fb-124">Hello Table szolgáltatásügyfél létrehozása</span><span class="sxs-lookup"><span data-stu-id="c76fb-124">Create hello Table service client</span></span>
<span data-ttu-id="c76fb-125">Hello [CloudTableClient] [ dotnet_CloudTableClient] osztály lehetővé teszi a Table storage-ban tárolt tooretrieve táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="c76fb-125">hello [CloudTableClient][dotnet_CloudTableClient] class enables you tooretrieve tables and entities stored in Table storage.</span></span> <span data-ttu-id="c76fb-126">Egyirányú toocreate hello Table szolgáltatásügyfél itt található:</span><span class="sxs-lookup"><span data-stu-id="c76fb-126">Here's one way toocreate hello Table service client:</span></span>

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

<span data-ttu-id="c76fb-127">Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooTable adattárolás áll.</span><span class="sxs-lookup"><span data-stu-id="c76fb-127">Now you are ready toowrite code that reads data from and writes data tooTable storage.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="c76fb-128">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="c76fb-128">Create a table</span></span>
<span data-ttu-id="c76fb-129">A példa bemutatja, hogyan toocreate egy táblát, ha még nem létezik:</span><span class="sxs-lookup"><span data-stu-id="c76fb-129">This example shows how toocreate a table if it does not already exist:</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Retrieve a reference toohello table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table if it doesn't exist.
table.CreateIfNotExists();
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="c76fb-130">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c76fb-130">Add an entity tooa table</span></span>
<span data-ttu-id="c76fb-131">Entitások tooC # objektumok leképezése egy származtatott egyéni osztály használatával [TableEntity][dotnet_TableEntity].</span><span class="sxs-lookup"><span data-stu-id="c76fb-131">Entities map tooC# objects by using a custom class derived from [TableEntity][dotnet_TableEntity].</span></span> <span data-ttu-id="c76fb-132">egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="c76fb-132">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="c76fb-133">hello alábbi kód meghatároz egy entitásosztályt, amely hello sorkulcsnak és a Vezetéknév hello partíció kulcsként hello ügyfél keresztnevét használja.</span><span class="sxs-lookup"><span data-stu-id="c76fb-133">hello following code defines an entity class that uses hello customer's first name as hello row key and last name as hello partition key.</span></span> <span data-ttu-id="c76fb-134">Együtt egy entitás partíció- és sorkulcsa egyedi azonosítása céljából hello táblában.</span><span class="sxs-lookup"><span data-stu-id="c76fb-134">Together, an entity's partition and row key uniquely identify it in hello table.</span></span> <span data-ttu-id="c76fb-135">Ugyanazzal a partíciókulccsal különböző rendelkező entitások gyorsabban lekérdezhetők, hello rendelkező entitások partícióazonosító kulcsok, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb méretezhetőségét teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="c76fb-135">Entities with hello same partition key can be queried faster than entities with different partition keys, but using diverse partition keys allows for greater scalability of parallel operations.</span></span> <span data-ttu-id="c76fb-136">Entitások toobe táblákban tárolt kell lennie, például származó hello támogatott típusú [TableEntity] [ dotnet_TableEntity] osztály.</span><span class="sxs-lookup"><span data-stu-id="c76fb-136">Entities toobe stored in tables must be of a supported type, for example derived from hello [TableEntity][dotnet_TableEntity] class.</span></span> <span data-ttu-id="c76fb-137">Azt szeretné, hogy a tábla toostore Entitástulajdonságok kell hello típus nyilvános tulajdonságait, és támogatja az első és az értékek beállítás.</span><span class="sxs-lookup"><span data-stu-id="c76fb-137">Entity properties you'd like toostore in a table must be public properties of hello type, and support both getting and setting of values.</span></span> <span data-ttu-id="c76fb-138">Az entitástípusnak emellett elérhetővé *kell* tennie egy paraméter nélküli konstruktort is.</span><span class="sxs-lookup"><span data-stu-id="c76fb-138">Also, your entity type *must* expose a parameter-less constructor.</span></span>

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

<span data-ttu-id="c76fb-139">Tábla, például az entitások műveleteket keresztül hello [CloudTable] [ dotnet_CloudTable] hello "Tábla létrehozása" szakaszban korábban létrehozott objektum.</span><span class="sxs-lookup"><span data-stu-id="c76fb-139">Table operations that involve entities are performed via hello [CloudTable][dotnet_CloudTable] object that you created earlier in hello "Create a table" section.</span></span> <span data-ttu-id="c76fb-140">hello végre műveletet toobe által képviselt egy [TableOperation] [ dotnet_TableOperation] objektum.</span><span class="sxs-lookup"><span data-stu-id="c76fb-140">hello operation toobe performed is represented by a [TableOperation][dotnet_TableOperation] object.</span></span> <span data-ttu-id="c76fb-141">hello alábbi példakód bemutatja hello hello létrehozása [CloudTable] [ dotnet_CloudTable] objektumot, majd egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="c76fb-141">hello following code example shows hello creation of hello [CloudTable][dotnet_CloudTable] object and then a **CustomerEntity** object.</span></span> <span data-ttu-id="c76fb-142">tooprepare hello műveletet, a [TableOperation] [ dotnet_TableOperation] objektum létrehozása tooinsert hello ügyfélentitást hello táblába.</span><span class="sxs-lookup"><span data-stu-id="c76fb-142">tooprepare hello operation, a [TableOperation][dotnet_TableOperation] object is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="c76fb-143">Végezetül hello művelet meghívásával hajtható végre [CloudTable][dotnet_CloudTable].[ Végrehajtás][dotnet_CloudTable_Execute].</span><span class="sxs-lookup"><span data-stu-id="c76fb-143">Finally, hello operation is executed by calling [CloudTable][dotnet_CloudTable].[Execute][dotnet_CloudTable_Execute].</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a new customer entity.
CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
customer1.Email = "Walter@contoso.com";
customer1.PhoneNumber = "425-555-0101";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer1);

// Execute hello insert operation.
table.Execute(insertOperation);
```

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="c76fb-144">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="c76fb-144">Insert a batch of entities</span></span>
<span data-ttu-id="c76fb-145">Egyetlen írási művelettel egy teljes entitásköteget is beszúrhat egy táblába.</span><span class="sxs-lookup"><span data-stu-id="c76fb-145">You can insert a batch of entities into a table in one write operation.</span></span> <span data-ttu-id="c76fb-146">Néhány további megjegyzés a kötegműveletekkel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="c76fb-146">Some other notes on batch operations:</span></span>

* <span data-ttu-id="c76fb-147">Frissítések, törlés és Beszúrás végezheti el a hello megegyezik egy kötegelt művelet.</span><span class="sxs-lookup"><span data-stu-id="c76fb-147">You can perform updates, deletes, and inserts in hello same single batch operation.</span></span>
* <span data-ttu-id="c76fb-148">Egy kötegművelet mentése too100 entitások tartalmazhatnak.</span><span class="sxs-lookup"><span data-stu-id="c76fb-148">A single batch operation can include up too100 entities.</span></span>
* <span data-ttu-id="c76fb-149">Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.</span><span class="sxs-lookup"><span data-stu-id="c76fb-149">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="c76fb-150">Bár ez a lekérdezés kötegelt műveletként lehetséges tooperform, hello hello köteg egyetlen művelete kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c76fb-150">While it is possible tooperform a query as a batch operation, it must be hello only operation in hello batch.</span></span>

<span data-ttu-id="c76fb-151">hello alábbi példakód létrehoz két entitásobjektumot, és hozzáadja minden egyes túl[TableBatchOperation] [ dotnet_TableBatchOperation] hello segítségével [beszúrása] [ dotnet_TableBatchOperation_Insert] módszer.</span><span class="sxs-lookup"><span data-stu-id="c76fb-151">hello following code example creates two entity objects and adds each too[TableBatchOperation][dotnet_TableBatchOperation] by using hello [Insert][dotnet_TableBatchOperation_Insert] method.</span></span> <span data-ttu-id="c76fb-152">Ezt követően [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] tooexecute hello művelet neve.</span><span class="sxs-lookup"><span data-stu-id="c76fb-152">Then, [CloudTable][dotnet_CloudTable].[ExecuteBatch][dotnet_CloudTable_ExecuteBatch] is called tooexecute hello operation.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello batch operation.
TableBatchOperation batchOperation = new TableBatchOperation();

// Create a customer entity and add it toohello table.
CustomerEntity customer1 = new CustomerEntity("Smith", "Jeff");
customer1.Email = "Jeff@contoso.com";
customer1.PhoneNumber = "425-555-0104";

// Create another customer entity and add it toohello table.
CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
customer2.Email = "Ben@contoso.com";
customer2.PhoneNumber = "425-555-0102";

// Add both customer entities toohello batch insert operation.
batchOperation.Insert(customer1);
batchOperation.Insert(customer2);

// Execute hello batch operation.
table.ExecuteBatch(batchOperation);
```

## <a name="retrieve-all-entities-in-a-partition"></a><span data-ttu-id="c76fb-153">Egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="c76fb-153">Retrieve all entities in a partition</span></span>
<span data-ttu-id="c76fb-154">egy tábla használja egy partíció összes entitását tooquery egy [TableQuery] [ dotnet_TableQuery] objektum.</span><span class="sxs-lookup"><span data-stu-id="c76fb-154">tooquery a table for all entities in a partition, use a [TableQuery][dotnet_TableQuery] object.</span></span> <span data-ttu-id="c76fb-155">hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások.</span><span class="sxs-lookup"><span data-stu-id="c76fb-155">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="c76fb-156">Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="c76fb-156">This example prints hello fields of each entity in hello query results toohello console.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Construct hello query operation for all customer entities where PartitionKey="Smith".
TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

// Print hello fields for each customer.
foreach (CustomerEntity entity in table.ExecuteQuery(query))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="c76fb-157">Partíció entitástartományának lekérése</span><span class="sxs-lookup"><span data-stu-id="c76fb-157">Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="c76fb-158">Egy partíció összes entitásának tooquery nem szeretné, ha egy tartományt is megadhat hello partíció kulcs szűrő egy sor szűrőjének kombinálásával.</span><span class="sxs-lookup"><span data-stu-id="c76fb-158">If you don't want tooquery all entities in a partition, you can specify a range by combining hello partition key filter with a row key filter.</span></span> <span data-ttu-id="c76fb-159">hello alábbi példakód két szűrő tooget összes entitásának a 'Smith', amelyen hello sorkulcs (Keresztnév) hello ábécé előtt "E" betűvel kezdődik, majd kinyomtatja hello lekérdezési eredmények partíció.</span><span class="sxs-lookup"><span data-stu-id="c76fb-159">hello following code example uses two filters tooget all entities in partition 'Smith' where hello row key (first name) starts with a letter before 'E' in hello alphabet, then prints hello query results.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create hello table query.
TableQuery<CustomerEntity> rangeQuery = new TableQuery<CustomerEntity>().Where(
    TableQuery.CombineFilters(
        TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"),
        TableOperators.And,
        TableQuery.GenerateFilterCondition("RowKey", QueryComparisons.LessThan, "E")));

// Loop through hello results, displaying information about hello entity.
foreach (CustomerEntity entity in table.ExecuteQuery(rangeQuery))
{
    Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
        entity.Email, entity.PhoneNumber);
}
```

## <a name="retrieve-a-single-entity"></a><span data-ttu-id="c76fb-160">Egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="c76fb-160">Retrieve a single entity</span></span>
<span data-ttu-id="c76fb-161">A lekérdezés tooretrieve írhat egy adott entitás.</span><span class="sxs-lookup"><span data-stu-id="c76fb-161">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="c76fb-162">hello következő kódban [TableOperation] [ dotnet_TableOperation] toospecify hello ügyfél "Ben Smith".</span><span class="sxs-lookup"><span data-stu-id="c76fb-162">hello following code uses [TableOperation][dotnet_TableOperation] toospecify hello customer 'Ben Smith'.</span></span> <span data-ttu-id="c76fb-163">Ez a módszer a gyűjtemény helyett csak egyetlen entitást adja vissza, és hello értéket adott vissza [TableResult][dotnet_TableResult].[ Eredmény] [ dotnet_TableResult_Result] van egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="c76fb-163">This method returns just one entity rather than a collection, and hello returned value in [TableResult][dotnet_TableResult].[Result][dotnet_TableResult_Result] is a **CustomerEntity** object.</span></span> <span data-ttu-id="c76fb-164">Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="c76fb-164">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Print hello phone number of hello result.
if (retrievedResult.Result != null)
{
    Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
}
else
{
    Console.WriteLine("hello phone number could not be retrieved.");
}
```

## <a name="replace-an-entity"></a><span data-ttu-id="c76fb-165">Entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="c76fb-165">Replace an entity</span></span>
<span data-ttu-id="c76fb-166">tooupdate egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági.</span><span class="sxs-lookup"><span data-stu-id="c76fb-166">tooupdate an entity, retrieve it from hello Table service, modify hello entity object, and then save hello changes back toohello Table service.</span></span> <span data-ttu-id="c76fb-167">hello alábbira vált egy meglévő ügyfél telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="c76fb-167">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="c76fb-168">Az [Insert][dotnet_TableOperation_Insert] parancs hívása helyett a kód a [Replace][dotnet_TableOperation_Replace] parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="c76fb-168">Instead of calling [Insert][dotnet_TableOperation_Insert], this code uses [Replace][dotnet_TableOperation_Replace].</span></span> <span data-ttu-id="c76fb-169">[Cserélje le] [ dotnet_TableOperation_Replace] okok hello entitás toobe hello kiszolgálón teljesen lecseréli, kivéve, ha a lekérdezés, amely esetben hello sikertelen lesz hello entitás hello kiszolgálón megváltozott.</span><span class="sxs-lookup"><span data-stu-id="c76fb-169">[Replace][dotnet_TableOperation_Replace] causes hello entity toobe fully replaced on hello server, unless hello entity on hello server has changed since it was retrieved, in which case hello operation will fail.</span></span> <span data-ttu-id="c76fb-170">Ez a hiba az alkalmazás ne írhasson felül véletlenül olyan módosítás történt közötti hello lekérését, és frissítse az alkalmazás egy másik összetevő tooprevent.</span><span class="sxs-lookup"><span data-stu-id="c76fb-170">This failure is tooprevent your application from inadvertently overwriting a change made between hello retrieval and update by another component of your application.</span></span> <span data-ttu-id="c76fb-171">hello Ez a hiba megfelelő kezeléséhez tooretrieve hello entitás újra, hajtsa végre a módosításokat (Ha még érvényesek), és ezután hajtson végre egy újabb [cserélje le] [ dotnet_TableOperation_Replace] műveletet.</span><span class="sxs-lookup"><span data-stu-id="c76fb-171">hello proper handling of this failure is tooretrieve hello entity again, make your changes (if still valid), and then perform another [Replace][dotnet_TableOperation_Replace] operation.</span></span> <span data-ttu-id="c76fb-172">hello a következő szakasz bemutatja, hogyan toooverride ezt a viselkedést.</span><span class="sxs-lookup"><span data-stu-id="c76fb-172">hello next section will show you how toooverride this behavior.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity object.
CustomerEntity updateEntity = (CustomerEntity)retrievedResult.Result;

if (updateEntity != null)
{
    // Change hello phone number.
    updateEntity.PhoneNumber = "425-555-0105";

    // Create hello Replace TableOperation.
    TableOperation updateOperation = TableOperation.Replace(updateEntity);

    // Execute hello operation.
    table.Execute(updateOperation);

    Console.WriteLine("Entity updated.");
}
else
{
    Console.WriteLine("Entity could not be retrieved.");
}
```

## <a name="insert-or-replace-an-entity"></a><span data-ttu-id="c76fb-173">Entitás beszúrása vagy lecserélése</span><span class="sxs-lookup"><span data-stu-id="c76fb-173">Insert-or-replace an entity</span></span>
<span data-ttu-id="c76fb-174">[Cserélje le] [ dotnet_TableOperation_Replace] műveletek sikertelenek lesznek, ha hello entitás hello kiszolgálóról lekérdezés óta módosult.</span><span class="sxs-lookup"><span data-stu-id="c76fb-174">[Replace][dotnet_TableOperation_Replace] operations will fail if hello entity has been changed since it was retrieved from hello server.</span></span> <span data-ttu-id="c76fb-175">Ezenkívül be kell olvasni hello entitás hello kiszolgálóról először ahhoz, hogy hello [cserélje le] [ dotnet_TableOperation_Replace] művelet toobe sikeres.</span><span class="sxs-lookup"><span data-stu-id="c76fb-175">Furthermore, you must retrieve hello entity from hello server first in order for hello [Replace][dotnet_TableOperation_Replace] operation toobe successful.</span></span> <span data-ttu-id="c76fb-176">Egyes esetekben azonban nem tudja Ha hello entitás létezik-e hello kiszolgálón, és hello a benne tárolt aktuális értékek irrelevánsak.</span><span class="sxs-lookup"><span data-stu-id="c76fb-176">Sometimes, however, you don't know if hello entity exists on hello server and hello current values stored in it are irrelevant.</span></span> <span data-ttu-id="c76fb-177">A frissítés mindent felülír.</span><span class="sxs-lookup"><span data-stu-id="c76fb-177">Your update should overwrite them all.</span></span> <span data-ttu-id="c76fb-178">tooaccomplish, szeretné használni egy [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] műveletet.</span><span class="sxs-lookup"><span data-stu-id="c76fb-178">tooaccomplish this, you would use an [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation.</span></span> <span data-ttu-id="c76fb-179">Ez a művelet beszúrja hello entitás, ha nem létezik, vagy ha igen, mikor hello utolsó frissítés történt függetlenül lecseréli.</span><span class="sxs-lookup"><span data-stu-id="c76fb-179">This operation inserts hello entity if it doesn't exist, or replaces it if it does, regardless of when hello last update was made.</span></span>

<span data-ttu-id="c76fb-180">Az alábbi kódpéldát hello egy ügyfélentitást "János János" a létrehozott és hello "személyek" táblába.</span><span class="sxs-lookup"><span data-stu-id="c76fb-180">In hello following code example, a customer entity for 'Fred Jones' is created and inserted into hello 'people' table.</span></span> <span data-ttu-id="c76fb-181">A következő használjuk hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] művelet toosave hello rendelkező entitás azonos partíciós kulcs (János) és a sor kulcs (János) toohello kiszolgáló, egy másik érték megadásával hello PhoneNumber most tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c76fb-181">Next, we use hello [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] operation toosave an entity with hello same partition key (Jones) and row key (Fred) toohello server, this time with a different value for hello PhoneNumber property.</span></span> <span data-ttu-id="c76fb-182">Mivel az [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] műveletet használjuk, minden tulajdonság értéke lecserélődik.</span><span class="sxs-lookup"><span data-stu-id="c76fb-182">Because we use [InsertOrReplace][dotnet_TableOperation_InsertOrReplace], all of its property values are replaced.</span></span> <span data-ttu-id="c76fb-183">Azonban ha egy "János János" entitás már hello tábla nem létezik, akkor volna beszúrását.</span><span class="sxs-lookup"><span data-stu-id="c76fb-183">However, if a 'Fred Jones' entity hadn't already existed in hello table, it would have been inserted.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable object that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a customer entity.
CustomerEntity customer3 = new CustomerEntity("Jones", "Fred");
customer3.Email = "Fred@contoso.com";
customer3.PhoneNumber = "425-555-0106";

// Create hello TableOperation object that inserts hello customer entity.
TableOperation insertOperation = TableOperation.Insert(customer3);

// Execute hello operation.
table.Execute(insertOperation);

// Create another customer entity with hello same partition key and row key.
// We've already created a 'Fred Jones' entity and saved it toothe
// 'people' table, but here we're specifying a different value for the
// PhoneNumber property.
CustomerEntity customer4 = new CustomerEntity("Jones", "Fred");
customer4.Email = "Fred@contoso.com";
customer4.PhoneNumber = "425-555-0107";

// Create hello InsertOrReplace TableOperation.
TableOperation insertOrReplaceOperation = TableOperation.InsertOrReplace(customer4);

// Execute hello operation. Because a 'Fred Jones' entity already exists in the
// 'people' table, its property values will be overwritten by those in this
// CustomerEntity. If 'Fred Jones' didn't already exist, hello entity would be
// added toohello table.
table.Execute(insertOrReplaceOperation);
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="c76fb-184">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="c76fb-184">Query a subset of entity properties</span></span>
<span data-ttu-id="c76fb-185">A lekérdezés pár tulajdonságok beolvasható egy entitás helyett minden hello entitás tulajdonságai.</span><span class="sxs-lookup"><span data-stu-id="c76fb-185">A table query can retrieve just a few properties from an entity instead of all hello entity properties.</span></span> <span data-ttu-id="c76fb-186">Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében.</span><span class="sxs-lookup"><span data-stu-id="c76fb-186">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="c76fb-187">a következő kód hello hello lekérdezést csak hello e-mail címek entitások hello tábla adja vissza.</span><span class="sxs-lookup"><span data-stu-id="c76fb-187">hello query in hello following code returns only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="c76fb-188">Ez a [DynamicTableEntity][dotnet_DynamicTableEntity] és az [EntityResolver][dotnet_EntityResolver] lekérdezésekkel hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="c76fb-188">This is done by using a query of [DynamicTableEntity][dotnet_DynamicTableEntity] and also [EntityResolver][dotnet_EntityResolver].</span></span> <span data-ttu-id="c76fb-189">A hello leképezése kapcsolatos részletesebb [Introducing Upsert és Query Projection blogbejegyzés][blog_post_upsert].</span><span class="sxs-lookup"><span data-stu-id="c76fb-189">You can learn more about projection in hello [Introducing Upsert and Query Projection blog post][blog_post_upsert].</span></span> <span data-ttu-id="c76fb-190">Leképezés nem támogatja hello storage emulatort, így a kód csak akkor, ha a fiók hello Table szolgáltatás használ.</span><span class="sxs-lookup"><span data-stu-id="c76fb-190">Projection is not supported by hello storage emulator, so this code runs only when you're using an account in hello Table service.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Define hello query, and select only hello Email property.
TableQuery<DynamicTableEntity> projectionQuery = new TableQuery<DynamicTableEntity>().Select(new string[] { "Email" });

// Define an entity resolver toowork with hello entity after retrieval.
EntityResolver<string> resolver = (pk, rk, ts, props, etag) => props.ContainsKey("Email") ? props["Email"].StringValue : null;

foreach (string projectedEmail in table.ExecuteQuery(projectionQuery, resolver, null, null))
{
    Console.WriteLine(projectedEmail);
}
```

## <a name="delete-an-entity"></a><span data-ttu-id="c76fb-191">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="c76fb-191">Delete an entity</span></span>
<span data-ttu-id="c76fb-192">Egyszerűen törölheti entitás hello segítségével beolvasása után egy entitás frissítésénél bemutatott minta.</span><span class="sxs-lookup"><span data-stu-id="c76fb-192">You can easily delete an entity after you have retrieved it by using hello same pattern shown for updating an entity.</span></span> <span data-ttu-id="c76fb-193">a következő kód hello kéri le, és töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="c76fb-193">hello following code retrieves and deletes a customer entity.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Create a retrieve operation that expects a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute hello operation.
TableResult retrievedResult = table.Execute(retrieveOperation);

// Assign hello result tooa CustomerEntity.
CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

// Create hello Delete TableOperation.
if (deleteEntity != null)
{
    TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

    // Execute hello operation.
    table.Execute(deleteOperation);

    Console.WriteLine("Entity deleted.");
}
else
{
    Console.WriteLine("Could not retrieve hello entity.");
}
```

## <a name="delete-a-table"></a><span data-ttu-id="c76fb-194">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="c76fb-194">Delete a table</span></span>
<span data-ttu-id="c76fb-195">Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="c76fb-195">Finally, hello following code example deletes a table from a storage account.</span></span> <span data-ttu-id="c76fb-196">A törölt tábla nem érhető el toobe egy adott időn belül hello törlését követően újra létre lesz.</span><span class="sxs-lookup"><span data-stu-id="c76fb-196">A table that has been deleted will be unavailable toobe re-created for a period of time following hello deletion.</span></span>

```csharp
// Retrieve hello storage account from hello connection string.
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
    CloudConfigurationManager.GetSetting("StorageConnectionString"));

// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

// Create hello CloudTable that represents hello "people" table.
CloudTable table = tableClient.GetTableReference("people");

// Delete hello table it if exists.
table.DeleteIfExists();
```

## <a name="retrieve-entities-in-pages-asynchronously"></a><span data-ttu-id="c76fb-197">Oldalak entitásainak aszinkron lekérése</span><span class="sxs-lookup"><span data-stu-id="c76fb-197">Retrieve entities in pages asynchronously</span></span>
<span data-ttu-id="c76fb-198">Ha sok entitást olvas, és a kívánt tooprocess/megjelenítendő entitások lehívás entitás összes tooreturn, akkor kérje le az entitásokat szegmentált lekérdezéssel helyett.</span><span class="sxs-lookup"><span data-stu-id="c76fb-198">If you are reading a large number of entities, and you want tooprocess/display entities as they are retrieved rather than waiting for them all tooreturn, you can retrieve entities by using a segmented query.</span></span> <span data-ttu-id="c76fb-199">Ez a példa bemutatja, hogyan tooreturn eredményezi, hogy közben nem blokkolja a végrehajtási hello Async-Await mintázat használatával lapok nagy számú eredmény tooreturn vár.</span><span class="sxs-lookup"><span data-stu-id="c76fb-199">This example shows how tooreturn results in pages by using hello Async-Await pattern so that execution is not blocked while you're waiting for a large set of results tooreturn.</span></span> <span data-ttu-id="c76fb-200">További információk az Async-Await mintázat a .NET hello, lásd: [aszinkron programozás az Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span><span class="sxs-lookup"><span data-stu-id="c76fb-200">For more details on using hello Async-Await pattern in .NET, see [Asynchronous programming with Async and Await (C# and Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).</span></span>

```csharp
// Initialize a default TableQuery tooretrieve all hello entities in hello table.
TableQuery<CustomerEntity> tableQuery = new TableQuery<CustomerEntity>();

// Initialize hello continuation token toonull toostart from hello beginning of hello table.
TableContinuationToken continuationToken = null;

do
{
    // Retrieve a segment (up too1,000 entities).
    TableQuerySegment<CustomerEntity> tableQueryResult =
        await table.ExecuteQuerySegmentedAsync(tableQuery, continuationToken);

    // Assign hello new continuation token tootell hello service where to
    // continue on hello next iteration (or null if it has reached hello end).
    continuationToken = tableQueryResult.ContinuationToken;

    // Print hello number of rows retrieved.
    Console.WriteLine("Rows retrieved {0}", tableQueryResult.Results.Count);

// Loop until a null continuation token is received, indicating hello end of hello table.
} while(continuationToken != null);
```

## <a name="next-steps"></a><span data-ttu-id="c76fb-201">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c76fb-201">Next steps</span></span>
<span data-ttu-id="c76fb-202">Most, hogy megismerte a Table storage alapjait hello, hajtsa végre az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn:</span><span class="sxs-lookup"><span data-stu-id="c76fb-202">Now that you've learned hello basics of Table storage, follow these links toolearn about more complex storage tasks:</span></span>

* <span data-ttu-id="c76fb-203">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="c76fb-203">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="c76fb-204">További Table Storage-példákat a [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel) című cikkben tekinthet meg.</span><span class="sxs-lookup"><span data-stu-id="c76fb-204">See more Table storage samples in [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/)</span></span>
* <span data-ttu-id="c76fb-205">Nézet hello tábla szolgáltatás elérhető API-kat vonatkozó dokumentációját:</span><span class="sxs-lookup"><span data-stu-id="c76fb-205">View hello Table service reference documentation for complete details about available APIs:</span></span>
* [<span data-ttu-id="c76fb-206">Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia</span><span class="sxs-lookup"><span data-stu-id="c76fb-206">Storage Client Library for .NET reference</span></span>](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [<span data-ttu-id="c76fb-207">REST API – referencia</span><span class="sxs-lookup"><span data-stu-id="c76fb-207">REST API reference</span></span>](http://msdn.microsoft.com/library/azure/dd179355)
* <span data-ttu-id="c76fb-208">Ismerje meg, hogyan toosimplify hello kódot ír hello segítségével az Azure Storage toowork [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="c76fb-208">Learn how toosimplify hello code you write toowork with Azure Storage by using hello [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)</span></span>
* <span data-ttu-id="c76fb-209">Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.</span><span class="sxs-lookup"><span data-stu-id="c76fb-209">View more feature guides toolearn about additional options for storing data in Azure.</span></span>
* <span data-ttu-id="c76fb-210">[Az Azure Blob storage .NET használatának első lépései](storage-dotnet-how-to-use-blobs.md) toostore strukturálatlan adatok.</span><span class="sxs-lookup"><span data-stu-id="c76fb-210">[Get started with Azure Blob storage using .NET](storage-dotnet-how-to-use-blobs.md) toostore unstructured data.</span></span>
* <span data-ttu-id="c76fb-211">[.NET (C#) használatával kapcsolódnak a tooSQL adatbázis](../sql-database/sql-database-develop-dotnet-simple.md) toostore relációs adatok.</span><span class="sxs-lookup"><span data-stu-id="c76fb-211">[Connect tooSQL Database by using .NET (C#)](../sql-database/sql-database-develop-dotnet-simple.md) toostore relational data.</span></span>

[Download and install hello Azure SDK for .NET]: /develop/net/
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
