---
title: "Ismerkedjen meg a table storage és a Visual Studio services (felhőszolgáltatások) kapcsolódó |} Microsoft Docs"
description: "Ismerkedés az Azure Table storage egy tárfiókot, a Visual Studio használatával csatlakozzon egy felhőszolgáltatás-projektet a Visual Studio használatával kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: TomArcher
manager: douge
editor: 
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: tarcher
ms.openlocfilehash: 3208ddb1a1246a5ff25d272bfc7d8ba842348a36
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="44705-103">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)</span><span class="sxs-lookup"><span data-stu-id="44705-103">Getting started with Azure table storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="44705-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="44705-104">Overview</span></span>
<span data-ttu-id="44705-105">Ez a cikk ismerteti az első lépések az Azure table storage a Visual Studióban létrehozott vagy a Visual Studio használatával Azure-tárfiók felhőalapú szolgáltatások projektben hivatkozott után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="44705-105">This article describes how to get started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using the Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="44705-106">A **kapcsolódó szolgáltatások hozzáadása** műveletet a megfelelő NuGet-csomagok az Azure storage a projekt eléréséhez telepíti, és a tárfiók kapcsolati karakterlánca ad hozzá a projekt konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="44705-106">The **Add Connected Services** operation installs the appropriate NuGet packages to access Azure storage in your project and adds the connection string for the storage account to your project configuration files.</span></span>

<span data-ttu-id="44705-107">Az Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű strukturált adatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="44705-107">The Azure Table storage service enables you to store large amounts of structured data.</span></span> <span data-ttu-id="44705-108">A szolgáltatás egy NoSQL-adattár, amely elfogadja az érkező hitelesített hívásokat belül és kívül az Azure felhőben.</span><span class="sxs-lookup"><span data-stu-id="44705-108">The service is a NoSQL datastore that accepts authenticated calls from inside and outside the Azure cloud.</span></span> <span data-ttu-id="44705-109">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="44705-109">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="44705-110">A kezdéshez, először a tárfiókban lévő tábla létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="44705-110">To get started, you first need to create a table in your storage account.</span></span> <span data-ttu-id="44705-111">Mutat be egy Azure-tábla létrehozása a kódban, valamint hogyan hajthat végre alapszintű tábla és entitás műveletek, például a hozzáadása, módosítása, olvasási és táblaentitásokat olvasásakor.</span><span class="sxs-lookup"><span data-stu-id="44705-111">We'll show you how to create an Azure table in code, and also how to perform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="44705-112">A Kódminták C nyelven íródtak\# code, és használja a [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="44705-112">The samples are written in C\# code and use the [Microsoft Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="44705-113">**Megjegyzés:** hajtsa végre az Azure storage hívásainak API-k aszinkron.</span><span class="sxs-lookup"><span data-stu-id="44705-113">**NOTE:** Some of the APIs that perform calls out to Azure storage are asynchronous.</span></span> <span data-ttu-id="44705-114">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="44705-114">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="44705-115">Az alábbi kódot azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="44705-115">The code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="44705-116">Lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md) bővebben programozott módon kezelését.</span><span class="sxs-lookup"><span data-stu-id="44705-116">See [Get started with Azure Table storage using .NET](storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.</span></span>
* <span data-ttu-id="44705-117">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="44705-117">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="44705-118">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="44705-118">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="44705-119">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="44705-119">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="44705-120">Hozzáférés táblák kódban</span><span class="sxs-lookup"><span data-stu-id="44705-120">Access tables in code</span></span>
<span data-ttu-id="44705-121">Hozzáférnek a cloud service projektek, C# forrásfájlokat, amelyhez hozzáférhetnek az Azure table storage a következő elemek is kell.</span><span class="sxs-lookup"><span data-stu-id="44705-121">To access tables in cloud service projects, you need to include the following items to any C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="44705-122">Győződjön meg arról, hogy a névtér-deklarációk, a C# fájlban tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="44705-122">Make sure the namespace declarations at the top of the C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="44705-123">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="44705-123">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="44705-124">A következő kódot használja a tárolási kapcsolati karakterlánc és a tárfiók adatait lekérése az Azure szolgáltatás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="44705-124">Use the following code to get the storage connection string and storage account information from the Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > <span data-ttu-id="44705-125">Az összes használja a fenti kódot a kód előtt a következő mintában.</span><span class="sxs-lookup"><span data-stu-id="44705-125">Use all of the above code in front of the code in the following samples.</span></span>
   > 
   > 
3. <span data-ttu-id="44705-126">Első egy **CloudTableClient** hivatkozhasson rá az a táblázat objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="44705-126">Get a **CloudTableClient** object to reference the table objects in your storage account.</span></span>
   
         // Create the table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="44705-127">Első egy **CloudTable** referenciaobjektum bizonyos táblájához és entitások mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="44705-127">Get a **CloudTable** reference object to reference a specific table and entities.</span></span>
   
        // Get a reference to a table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="44705-128">Tábla létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="44705-128">Create a table in code</span></span>
<span data-ttu-id="44705-129">Az Azure-tábla létrehozásához csak adjon hozzá egy **CreateIfNotExistsAsync** való a után kap egy **CloudTable** objektum a hozzáférés-kódban táblák"című részben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="44705-129">To create the Azure table, just add a call to **CreateIfNotExistsAsync** to the after you get a **CloudTable** object as described in the "Access tables in code" section.</span></span>

    // Create the CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="44705-130">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="44705-130">Add an entity to a table</span></span>
<span data-ttu-id="44705-131">Ha hozzá szeretne adni egy entitást egy táblához, hozzon létre egy osztályt, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="44705-131">To add an entity to a table, create a class that defines the properties of your entity.</span></span> <span data-ttu-id="44705-132">Az alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , amely használ az ügyfél keresztnevét a sorkulcs és a vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="44705-132">The following code defines an entity class called **CustomerEntity** that uses the customer's first name as the row key and the last name as the partition key.</span></span>

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

<span data-ttu-id="44705-133">Entitások érintő tábla műveletet végzett használatával a **CloudTable** objektum, amely a korábban létrehozott "Hozzáférési táblák a kódban."</span><span class="sxs-lookup"><span data-stu-id="44705-133">Table operations involving entities are done using the **CloudTable** object that you created earlier in "Access tables in code."</span></span> <span data-ttu-id="44705-134">A **TableOperation** objektum által jelképezett kell elvégezni a műveletet.</span><span class="sxs-lookup"><span data-stu-id="44705-134">The **TableOperation** object represents the operation to be done.</span></span> <span data-ttu-id="44705-135">Az alábbi példakód bemutatja, hogyan hozzon létre egy **CloudTable** objektum és a **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="44705-135">The following code example shows how to create a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="44705-136">A művelet előkészítéséhez egy **TableOperation** jön létre, beszúrja az ügyfélentitást a táblába.</span><span class="sxs-lookup"><span data-stu-id="44705-136">To prepare the operation, a **TableOperation** is created to insert the customer entity into the table.</span></span> <span data-ttu-id="44705-137">Végezetül, a művelet végrehajtása meghívásával **CloudTable.ExecuteAsync**.</span><span class="sxs-lookup"><span data-stu-id="44705-137">Finally, the operation is executed by calling **CloudTable.ExecuteAsync**.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create the TableOperation that inserts the customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute the insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="44705-138">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="44705-138">Insert a batch of entities</span></span>
<span data-ttu-id="44705-139">Több entitás egyetlen írási művelettel egy táblába beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="44705-139">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="44705-140">Az alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), hozzáadja őket egy **TableBatchOperation** objektumot, az Insert metódus használata, és ezután elindítja a művelet által hívott  **CloudTable.ExecuteBatchAsync**.</span><span class="sxs-lookup"><span data-stu-id="44705-140">The following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them to a **TableBatchOperation** object using the Insert method, and then starts the operation by calling **CloudTable.ExecuteBatchAsync**.</span></span>

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

## <a name="get-all-of-the-entities-in-a-partition"></a><span data-ttu-id="44705-141">Egy partíció beolvasása összes entitások</span><span class="sxs-lookup"><span data-stu-id="44705-141">Get all of the entities in a partition</span></span>
<span data-ttu-id="44705-142">Egy tábla összes partíció entitástartományának lekérdezéséhez használja a **TableQuery** objektum.</span><span class="sxs-lookup"><span data-stu-id="44705-142">To query a table for all of the entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="44705-143">Az alábbi példakód megad egy szűrőt a „Smith” partíciókulcsú entitásokra.</span><span class="sxs-lookup"><span data-stu-id="44705-143">The following code example specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="44705-144">A példa megjeleníti a konzolon a lekérdezés eredményei között szereplő entitásokhoz tartozó mezőket.</span><span class="sxs-lookup"><span data-stu-id="44705-144">This example prints the fields of each entity in the query results to the console.</span></span>

    // Construct the query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

    return View();


## <a name="get-a-single-entity"></a><span data-ttu-id="44705-145">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="44705-145">Get a single entity</span></span>
<span data-ttu-id="44705-146">Írhat egy lekérdezést egy adott entitás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="44705-146">You can write a query to get a single, specific entity.</span></span> <span data-ttu-id="44705-147">A következő kódban a **TableOperation** adhatja meg az ügyfél "Ben Smith" nevű objektum.</span><span class="sxs-lookup"><span data-stu-id="44705-147">The following code uses a **TableOperation** object to specify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="44705-148">Ez a módszer adja vissza, csak egyetlen entitást helyett egy gyűjtemény és a visszaadott érték **Ableresult.Result** van egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="44705-148">This method returns just one entity, rather than a collection, and the returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="44705-149">A leggyorsabban az egyetlen entitás lekérdezése partíció-és sorkulcsok megadása a lekérdezésben a **tábla** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="44705-149">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute the retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print the phone number of the result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("The phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="44705-150">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="44705-150">Delete an entity</span></span>
<span data-ttu-id="44705-151">Miután megtalálta az entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="44705-151">You can delete an entity after you find it.</span></span> <span data-ttu-id="44705-152">A következő kód egy ügyfélentitást "Ben Smith" nevű keres, és megtalálja, ha azt törli őket.</span><span class="sxs-lookup"><span data-stu-id="44705-152">The following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="44705-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="44705-153">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

