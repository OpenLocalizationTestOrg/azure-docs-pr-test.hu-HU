---
title: "Első lépések a Visual Studio és a table storage (az ASP.NET Core) szolgáltatások csatlakoztatott |} Microsoft Docs"
description: "Ismerkedés az Azure Table storage egy ASP.NET Core projekt, a Visual Studio egy tárfiókot, a Visual Studio használatával történő kapcsolódás után kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: b64d4f7e55977c7ce144987f7600e5ddcb25596c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-get-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="2a83b-103">Ismerkedés az Azure Table storage és a Visual Studio kapcsolódó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2a83b-103">How to get started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="2a83b-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2a83b-104">Overview</span></span>
<span data-ttu-id="2a83b-105">Ez a cikk ismerteti, hogyan Ismerkedés az Azure Table storage a Visual Studióban létrehozott vagy a Visual Studio használatával Azure-tárfiók az ASP.NET Core projekt hivatkozik után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2a83b-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using the Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="2a83b-106">Az Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű strukturált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="2a83b-106">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="2a83b-107">A szolgáltatás egy nosql-alapú adattároló, amely elfogadja az érkező hitelesített hívásokat belül és kívül az Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="2a83b-107">The service is a NoSQL data store that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="2a83b-108">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="2a83b-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="2a83b-109">A **kapcsolódó szolgáltatások hozzáadása** műveletet a megfelelő NuGet-csomagok az Azure storage a projekt eléréséhez telepíti, és a tárfiók kapcsolati karakterlánca ad hozzá a projekt konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2a83b-109">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="2a83b-110">Azure Table storage használatával kapcsolatos további általános információkért lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="2a83b-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="2a83b-111">A kezdéshez, először a tárfiókban lévő tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2a83b-111">To get started, you first need to create a table in your storage account.</span></span> <span data-ttu-id="2a83b-112">Mutat be egy Azure-tábla létrehozása a kódban.</span><span class="sxs-lookup"><span data-stu-id="2a83b-112">We'll show you how to create an Azure table in code.</span></span> <span data-ttu-id="2a83b-113">Is mutat be, hogyan hajthat végre alapszintű tábla és entitás műveletek, például a hozzáadása, módosítása, olvasási és táblaentitásokat olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="2a83b-113">We'll also show you how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="2a83b-114">A Kódminták C nyelven íródtak\# code, és használja az Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="2a83b-114">The samples are written in C\# code and use the Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="2a83b-115">**Megjegyzés:** -hajtsa végre az Azure storage hívásainak ASP.NET Core API-k aszinkron.</span><span class="sxs-lookup"><span data-stu-id="2a83b-115">**NOTE** - Some of the APIs that perform calls out to Azure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="2a83b-116">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="2a83b-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="2a83b-117">Az alábbi kódot azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="2a83b-117">The code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="2a83b-118">Hozzáférés táblák kódban</span><span class="sxs-lookup"><span data-stu-id="2a83b-118">Access tables in code</span></span>
<span data-ttu-id="2a83b-119">Az ASP.NET Core projektek hozzáférnek, C# forrásfájlokat, amelyhez hozzáférhetnek az Azure table storage a következő elemek is kell.</span><span class="sxs-lookup"><span data-stu-id="2a83b-119">To access tables in ASP.NET Core projects, you need to include the following items to any C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="2a83b-120">Győződjön meg arról, hogy a névtér-deklarációk, a C# fájlban tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2a83b-120">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="2a83b-121">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="2a83b-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="2a83b-122">A következő kód segítségével beolvasása a a tárolási kapcsolati karakterlánc és a tárfiók adatait az Azure-szolgáltatások konfigurációjából.</span><span class="sxs-lookup"><span data-stu-id="2a83b-122">Use the following code to get the your storage connection string and storage account information from the Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="2a83b-123">**Megjegyzés:** -összes a fenti kódot a kód előtt használja a következő mintában.</span><span class="sxs-lookup"><span data-stu-id="2a83b-123">**NOTE** - Use all of the above code in front of the code in the following samples.</span></span>
3. <span data-ttu-id="2a83b-124">Első egy **CloudTableClient** hivatkozhasson rá az a táblázat objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="2a83b-124">Get a **CloudTableClient** object to reference the table objects in your storage account.</span></span>  
   
        // Create the table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="2a83b-125">Első egy **CloudTable** referenciaobjektum bizonyos táblájához és entitások mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="2a83b-125">Get a **CloudTable** reference object to reference a specific table and entities.</span></span>
   
        // Get a reference to a table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="2a83b-126">Tábla létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="2a83b-126">Create a table in code</span></span>
<span data-ttu-id="2a83b-127">Az Azure-tábla létrehozásához csak adjon hozzá egy **CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="2a83b-127">To create the Azure table, just add a call to **CreateIfNotExistsAsync()**.</span></span>

    // Create the CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="2a83b-128">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="2a83b-128">Add an entity to a table</span></span>
<span data-ttu-id="2a83b-129">Egy entitás hozzáadása egy táblát hozzon létre egy osztályt, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="2a83b-129">To add an entity to a table you create a class that defines the properties of your entity.</span></span> <span data-ttu-id="2a83b-130">Az alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , amely használ az ügyfél keresztnevét sorkulcsnak és a vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="2a83b-130">The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and last name as the partition key.</span></span>

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

<span data-ttu-id="2a83b-131">Entitások érintő tábla műveletet végzett használatával a **CloudTable** objektumot, amelyet korábban létrehozott "Hozzáférési táblák a kódban."</span><span class="sxs-lookup"><span data-stu-id="2a83b-131">Table operations involving entities are done using the **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="2a83b-132">A **TableOperation** objektum által jelképezett kell elvégezni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="2a83b-132">The **TableOperation** object represents the operation to be done.</span></span> <span data-ttu-id="2a83b-133">Az alábbi példakód bemutatja, hogyan hozzon létre egy **CloudTable** objektum és a **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="2a83b-133">The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="2a83b-134">A művelet előkészítéséhez egy **TableOperation** jön létre, beszúrja az ügyfélentitást a táblába.</span><span class="sxs-lookup"><span data-stu-id="2a83b-134">To prepare the operation, a **TableOperation** is created to insert the customer entity into the table.</span></span> <span data-ttu-id="2a83b-135">Végezetül a művelet CloudTable.ExecuteAsync meghívásával hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="2a83b-135">Finally, the operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="2a83b-136">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="2a83b-136">Insert a batch of entities</span></span>
<span data-ttu-id="2a83b-137">Több entitás egyetlen írási művelettel egy táblába beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="2a83b-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="2a83b-138">Az alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), hozzáadja őket egy **TableBatchOperation** objektumba a **beszúrása** módszer, majd elindítja a művelet által hívott CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="2a83b-138">The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the **Insert** method, and then starts the operation by calling CloudTable.ExecuteBatchAsync.</span></span>

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-the-entities-in-a-partition"></a><span data-ttu-id="2a83b-139">Egy partíció beolvasása összes entitások</span><span class="sxs-lookup"><span data-stu-id="2a83b-139">Get all of the entities in a partition</span></span>
<span data-ttu-id="2a83b-140">Egy tábla összes partíció entitástartományának lekérdezéséhez használja a **TableQuery** objektum.</span><span class="sxs-lookup"><span data-stu-id="2a83b-140">To query a table for all of the entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="2a83b-141">Az alábbi példakód megad egy szűrőt a „Smith” partíciókulcsú entitásokra.</span><span class="sxs-lookup"><span data-stu-id="2a83b-141">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="2a83b-142">A példa megjeleníti a konzolon a lekérdezés eredményei között szereplő entitásokhoz tartozó mezőket.</span><span class="sxs-lookup"><span data-stu-id="2a83b-142">This example prints the fields of each entity in the query results to the console.</span></span>

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print the fields for each customer.
    TableContinuationToken token = null;
    do
    {
        TableQuerySegment<CustomerEntity> resultSegment = await peopleTable.ExecuteQuerySegmentedAsync(query, token);
        token = resultSegment.ContinuationToken;

        foreach (CustomerEntity entity in resultSegment.Results)
        {
            Console.WriteLine("{0}, {1}\t{2}\t{3}", entity.PartitionKey, entity.RowKey,
            entity.Email, entity.PhoneNumber);
        }
    } while (token != null);

## <a name="get-a-single-entity"></a><span data-ttu-id="2a83b-143">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="2a83b-143">Get a single entity</span></span>
<span data-ttu-id="2a83b-144">Írhat egy lekérdezést egy adott entitás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2a83b-144">You can write a query to get a single, specific entity.</span></span> <span data-ttu-id="2a83b-145">A következő kódban a **TableOperation** adhatja meg az ügyfél "Ben Smith" nevű objektum.</span><span class="sxs-lookup"><span data-stu-id="2a83b-145">The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="2a83b-146">Ez a módszer adja vissza, csak egyetlen entitást helyett egy gyűjtemény és a visszaadott érték **Ableresult.Result** van egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="2a83b-146">This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="2a83b-147">A leggyorsabban az egyetlen entitás lekérdezése partíció-és sorkulcsok megadása a lekérdezésben a **tábla** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2a83b-147">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="2a83b-148">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="2a83b-148">Delete an entity</span></span>
<span data-ttu-id="2a83b-149">Miután megtalálta az entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="2a83b-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="2a83b-150">A következő kód egy ügyfélentitást "Ben Smith" nevű keres, és megtalálja, ha azt törli őket.</span><span class="sxs-lookup"><span data-stu-id="2a83b-150">The following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign the result to a CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create the Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute the operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete the entity.");

## <a name="next-steps"></a><span data-ttu-id="2a83b-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a83b-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

