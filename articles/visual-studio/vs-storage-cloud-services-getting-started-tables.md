---
title: "a table storage és a Visual Studio (felhőszolgáltatások) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának Azure Table storage egy Visual Studio felhőszolgáltatás-projektet a Visual Studio használatával tooa tárfiók kapcsolódás szolgáltatások csatlakoztatása után"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: a3a11ed8-ba7f-4193-912b-e555f5b72184
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 36da6ed4a12a3595e7234482e3040ecee8c33b8a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a><span data-ttu-id="cf219-103">Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)</span><span class="sxs-lookup"><span data-stu-id="cf219-103">Getting started with Azure table storage and Visual Studio connected services (cloud services projects)</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="cf219-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="cf219-104">Overview</span></span>
<span data-ttu-id="cf219-105">Ez a cikk ismerteti, hogyan tooget el az Azure table storage a Visual Studióban létrehozott vagy egy Azure storage-fiók felhőalapú szolgáltatások projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel .</span><span class="sxs-lookup"><span data-stu-id="cf219-105">This article describes how tooget started using Azure table storage in Visual Studio after you have created or referenced an Azure storage account in a cloud services project by using hello Visual Studio **Add Connected Services** dialog.</span></span> <span data-ttu-id="cf219-106">Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="cf219-106">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="cf219-107">hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="cf219-107">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="cf219-108">hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="cf219-108">hello service is a NoSQL datastore that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="cf219-109">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="cf219-109">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="cf219-110">tooget elindult, először toocreate egy táblát a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="cf219-110">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="cf219-111">Mutat be, hogyan toocreate az Azure table-kódban, valamint hogyan tooperform egyszerű táblázat és entitás műveletek, például a hozzáadása, módosítása, olvasása, és olvasási tábla entitások.</span><span class="sxs-lookup"><span data-stu-id="cf219-111">We'll show you how toocreate an Azure table in code, and also how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="cf219-112">hello minták C nyelven íródtak\# code és hello használata [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span><span class="sxs-lookup"><span data-stu-id="cf219-112">hello samples are written in C\# code and use hello [Microsoft Azure Storage client library for .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).</span></span>

<span data-ttu-id="cf219-113">**Megjegyzés:** hello API-k, hogy tooAzure tároló hívások végző aszinkron.</span><span class="sxs-lookup"><span data-stu-id="cf219-113">**NOTE:** Some of hello APIs that perform calls out tooAzure storage are asynchronous.</span></span> <span data-ttu-id="cf219-114">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="cf219-114">See [Asynchronous programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="cf219-115">az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="cf219-115">hello code below assumes async programming methods are being used.</span></span>

* <span data-ttu-id="cf219-116">Lásd: [Ismerkedés az Azure Table storage használatának .NET](../storage/storage-dotnet-how-to-use-tables.md) bővebben programozott módon kezelését.</span><span class="sxs-lookup"><span data-stu-id="cf219-116">See [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md) for more information on programmatically manipulating tables.</span></span>
* <span data-ttu-id="cf219-117">Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="cf219-117">See [Storage documentation](https://azure.microsoft.com/documentation/services/storage/) for general information about Azure Storage.</span></span>
* <span data-ttu-id="cf219-118">Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.</span><span class="sxs-lookup"><span data-stu-id="cf219-118">See [Cloud Services documentation](https://azure.microsoft.com/documentation/services/cloud-services/) for general information about Azure cloud services.</span></span>
* <span data-ttu-id="cf219-119">Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.</span><span class="sxs-lookup"><span data-stu-id="cf219-119">See [ASP.NET](http://www.asp.net) for more information about programming ASP.NET applications.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="cf219-120">Hozzáférés táblák kódban</span><span class="sxs-lookup"><span data-stu-id="cf219-120">Access tables in code</span></span>
<span data-ttu-id="cf219-121">cloud service projektek tooaccess táblák, a következő elemek tooany C# forrásfájlok Azure table storage elérő tooinclude hello kell.</span><span class="sxs-lookup"><span data-stu-id="cf219-121">tooaccess tables in cloud service projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="cf219-122">Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="cf219-122">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="cf219-123">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="cf219-123">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="cf219-124">Kód tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait következő hello Azure szolgáltatáskonfiguráció hello használata.</span><span class="sxs-lookup"><span data-stu-id="cf219-124">Use hello following code tooget hello storage connection string and storage account information from hello Azure service configuration.</span></span>
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > <span data-ttu-id="cf219-125">A következő minták hello az összes fenti kódot hello kód előtt hello használja.</span><span class="sxs-lookup"><span data-stu-id="cf219-125">Use all of hello above code in front of hello code in hello following samples.</span></span>
   > 
   > 
3. <span data-ttu-id="cf219-126">Első egy **CloudTableClient** tooreference hello tábla objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="cf219-126">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>
   
         // Create hello table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="cf219-127">Első egy **CloudTable** objektum tooreference bizonyos táblájához és entitások.</span><span class="sxs-lookup"><span data-stu-id="cf219-127">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="cf219-128">Tábla létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="cf219-128">Create a table in code</span></span>
<span data-ttu-id="cf219-129">toocreate hello Azure-tábla csak adjon hozzá egy túl**CreateIfNotExistsAsync** toohello után kap egy **CloudTable** objektum hello "Code Access táblák" című részben leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="cf219-129">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync** toohello after you get a **CloudTable** object as described in hello "Access tables in code" section.</span></span>

    // Create hello CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="cf219-130">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cf219-130">Add an entity tooa table</span></span>
<span data-ttu-id="cf219-131">egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="cf219-131">tooadd an entity tooa table, create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="cf219-132">hello alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és hello vezetékneve hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="cf219-132">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and hello last name as hello partition key.</span></span>

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

<span data-ttu-id="cf219-133">Entitások érintő tábla műveletet végzett hello segítségével **CloudTable** objektum, amely a korábban létrehozott "Hozzáférési táblák a kódban."</span><span class="sxs-lookup"><span data-stu-id="cf219-133">Table operations involving entities are done using hello **CloudTable** object that you created earlier in "Access tables in code."</span></span> <span data-ttu-id="cf219-134">Hello **TableOperation** objektum által jelképezett hello művelet toobe történik.</span><span class="sxs-lookup"><span data-stu-id="cf219-134">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="cf219-135">a következő kód példa azt mutatja meg hogyan hello toocreate egy **CloudTable** objektum és a **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="cf219-135">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="cf219-136">tooprepare hello műveletet, a **TableOperation** hello táblába tooinsert hello ügyfélentitást jön létre.</span><span class="sxs-lookup"><span data-stu-id="cf219-136">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="cf219-137">Végezetül hello művelet meghívásával hajtható végre **CloudTable.ExecuteAsync**.</span><span class="sxs-lookup"><span data-stu-id="cf219-137">Finally, hello operation is executed by calling **CloudTable.ExecuteAsync**.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="cf219-138">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="cf219-138">Insert a batch of entities</span></span>
<span data-ttu-id="cf219-139">Több entitás egyetlen írási művelettel egy táblába beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="cf219-139">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="cf219-140">hello alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), azokat beveszi tooa **TableBatchOperation** objektumba hello Insert metódus, és ezután elindítja hello meghívásával művelet  **CloudTable.ExecuteBatchAsync**.</span><span class="sxs-lookup"><span data-stu-id="cf219-140">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello Insert method, and then starts hello operation by calling **CloudTable.ExecuteBatchAsync**.</span></span>

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
    await peopleTable.ExecuteBatchAsync(batchOperation);

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="cf219-141">Egy partíció beolvasása összes hello entitások</span><span class="sxs-lookup"><span data-stu-id="cf219-141">Get all of hello entities in a partition</span></span>
<span data-ttu-id="cf219-142">egy tábla összes hello entitások egy partíció használjon tooquery egy **TableQuery** objektum.</span><span class="sxs-lookup"><span data-stu-id="cf219-142">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="cf219-143">hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások.</span><span class="sxs-lookup"><span data-stu-id="cf219-143">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="cf219-144">Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="cf219-144">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
        .Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

    // Print hello fields for each customer.
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


## <a name="get-a-single-entity"></a><span data-ttu-id="cf219-145">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="cf219-145">Get a single entity</span></span>
<span data-ttu-id="cf219-146">A lekérdezés tooget írhat egy adott entitás.</span><span class="sxs-lookup"><span data-stu-id="cf219-146">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="cf219-147">hello következő kódban a **TableOperation** toospecify az ügyfél "Ben Smith" nevű objektum.</span><span class="sxs-lookup"><span data-stu-id="cf219-147">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="cf219-148">Ez a módszer értéket ad vissza, egy gyűjtemény helyett csak egyetlen entitást, és hello értéket adott vissza **Ableresult.Result** van egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="cf219-148">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="cf219-149">Hello leggyorsabb módon tooretrieve hello az egyetlen entitás partíció-és sorkulcsok megadása a lekérdezésben **tábla** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cf219-149">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="cf219-150">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="cf219-150">Delete an entity</span></span>
<span data-ttu-id="cf219-151">Miután megtalálta az entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="cf219-151">You can delete an entity after you find it.</span></span> <span data-ttu-id="cf219-152">hello alábbira keres egy ügyfélentitást "Ben Smith" nevű, és megtalálja, ha azt törli őket.</span><span class="sxs-lookup"><span data-stu-id="cf219-152">hello following code looks for a customer entity named "Ben Smith", and if it finds it, it deletes it.</span></span>

    // Create a retrieve operation that expects a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello operation.
    TableResult retrievedResult = peopleTable.Execute(retrieveOperation);

    // Assign hello result tooa CustomerEntity object.
    CustomerEntity deleteEntity = (CustomerEntity)retrievedResult.Result;

    // Create hello Delete TableOperation and then execute it.
    if (deleteEntity != null)
    {
       TableOperation deleteOperation = TableOperation.Delete(deleteEntity);

       // Execute hello operation.
       await peopleTable.ExecuteAsync(deleteOperation);

       Console.WriteLine("Entity deleted.");
    }

    else
       Console.WriteLine("Couldn't delete hello entity.");

## <a name="next-steps"></a><span data-ttu-id="cf219-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf219-153">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

