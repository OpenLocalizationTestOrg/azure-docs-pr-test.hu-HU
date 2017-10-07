---
title: "a table storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaHow tooget |} Microsoft Docs"
description: "Hogyan tooget után indítják el, az ASP.NET Core projektben a Visual Studio Azure Table storage csatlakozás a Visual Studio használatával tooa tárfiók kapcsolódó szolgáltatások"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: c3c451d1-71ff-4222-a348-c41c98a02b85
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: e3eb3f3e65456108dd3cde7e3e470f98ba456e35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a><span data-ttu-id="45e3c-103">Hogyan tooget el az Azure Table storage és a Visual Studio kapcsolódó szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="45e3c-103">How tooget started with Azure Table storage and Visual Studio connected services</span></span>
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a><span data-ttu-id="45e3c-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="45e3c-104">Overview</span></span>
<span data-ttu-id="45e3c-105">Ez a cikk ismerteti, hogyan használatának Azure Table storage a Visual Studio után készített vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott használatával hello Visual Studio **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="45e3c-105">This article describes how get started using Azure Table storage in Visual Studio after you have created or referenced an Azure storage account in an ASP.NET Core project by using hello Visual Studio **Add Connected Services** dialog.</span></span>

<span data-ttu-id="45e3c-106">hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok.</span><span class="sxs-lookup"><span data-stu-id="45e3c-106">hello Azure Table storage service enables you toostore large amounts of structured data.</span></span> <span data-ttu-id="45e3c-107">hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja.</span><span class="sxs-lookup"><span data-stu-id="45e3c-107">hello service is a NoSQL data store that accepts authenticated calls from inside and outside hello Azure cloud.</span></span> <span data-ttu-id="45e3c-108">Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.</span><span class="sxs-lookup"><span data-stu-id="45e3c-108">Azure tables are ideal for storing structured, non-relational data.</span></span>

<span data-ttu-id="45e3c-109">Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.</span><span class="sxs-lookup"><span data-stu-id="45e3c-109">hello **Add Connected Services** operation installs hello appropriate NuGet packages tooaccess Azure storage in your project and adds hello connection string for hello storage account tooyour project configuration files.</span></span>

<span data-ttu-id="45e3c-110">Azure Table storage használatával kapcsolatos további általános információkért lásd: [Ismerkedés az Azure Table storage használatának .NET](../storage/storage-dotnet-how-to-use-tables.md).</span><span class="sxs-lookup"><span data-stu-id="45e3c-110">For more general information about using Azure Table storage, see [Get started with Azure Table storage using .NET](../storage/storage-dotnet-how-to-use-tables.md).</span></span>

<span data-ttu-id="45e3c-111">tooget elindult, először toocreate egy táblát a tárfiókban lévő.</span><span class="sxs-lookup"><span data-stu-id="45e3c-111">tooget started, you first need toocreate a table in your storage account.</span></span> <span data-ttu-id="45e3c-112">Mutat be, hogyan toocreate az Azure table-kódban.</span><span class="sxs-lookup"><span data-stu-id="45e3c-112">We'll show you how toocreate an Azure table in code.</span></span> <span data-ttu-id="45e3c-113">Is mutat be, hogyan tooperform egyszerű táblázat és entitás műveletek, például a hozzáadása, módosítása, olvasása, és olvasási tábla entitásokat.</span><span class="sxs-lookup"><span data-stu-id="45e3c-113">We'll also show you how tooperform basic table and entity operations, such as adding, modifying, reading and reading table entities.</span></span> <span data-ttu-id="45e3c-114">hello minták C nyelven íródtak\# code, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez.</span><span class="sxs-lookup"><span data-stu-id="45e3c-114">hello samples are written in C\# code and use hello Azure Storage Client Library for .NET.</span></span>

<span data-ttu-id="45e3c-115">**Megjegyzés:** -hello API-hívások, hogy tooAzure tároló hajtsa végre az ASP.NET Core aszinkron.</span><span class="sxs-lookup"><span data-stu-id="45e3c-115">**NOTE** - Some of hello APIs that perform calls out tooAzure storage in ASP.NET Core are asynchronous.</span></span> <span data-ttu-id="45e3c-116">Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt.</span><span class="sxs-lookup"><span data-stu-id="45e3c-116">See [Asynchronous Programming with Async and Await](http://msdn.microsoft.com/library/hh191443.aspx) for more information.</span></span> <span data-ttu-id="45e3c-117">az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.</span><span class="sxs-lookup"><span data-stu-id="45e3c-117">hello code below assumes Async programming methods are being used.</span></span>

## <a name="access-tables-in-code"></a><span data-ttu-id="45e3c-118">Hozzáférés táblák kódban</span><span class="sxs-lookup"><span data-stu-id="45e3c-118">Access tables in code</span></span>
<span data-ttu-id="45e3c-119">ASP.NET Core projektek tooaccess táblák, a következő elemek tooany C# forrásfájlok Azure table storage elérő tooinclude hello kell.</span><span class="sxs-lookup"><span data-stu-id="45e3c-119">tooaccess tables in ASP.NET Core projects, you need tooinclude hello following items tooany C# source files that access Azure table storage.</span></span>

1. <span data-ttu-id="45e3c-120">Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.</span><span class="sxs-lookup"><span data-stu-id="45e3c-120">Make sure hello namespace declarations at hello top of hello C# file include these **using** statements.</span></span>
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. <span data-ttu-id="45e3c-121">Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli.</span><span class="sxs-lookup"><span data-stu-id="45e3c-121">Get a **CloudStorageAccount** object that represents your storage account information.</span></span> <span data-ttu-id="45e3c-122">A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.</span><span class="sxs-lookup"><span data-stu-id="45e3c-122">Use hello following code tooget hello your storage connection string and storage account information from hello Azure service configuration.</span></span>
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    <span data-ttu-id="45e3c-123">**Megjegyzés:** -a következő minták hello az összes fenti kódot hello kód előtt hello használja.</span><span class="sxs-lookup"><span data-stu-id="45e3c-123">**NOTE** - Use all of hello above code in front of hello code in hello following samples.</span></span>
3. <span data-ttu-id="45e3c-124">Első egy **CloudTableClient** tooreference hello tábla objektumok a tárfiókban lévő objektum.</span><span class="sxs-lookup"><span data-stu-id="45e3c-124">Get a **CloudTableClient** object tooreference hello table objects in your storage account.</span></span>  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. <span data-ttu-id="45e3c-125">Első egy **CloudTable** objektum tooreference bizonyos táblájához és entitások.</span><span class="sxs-lookup"><span data-stu-id="45e3c-125">Get a **CloudTable** reference object tooreference a specific table and entities.</span></span>
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a><span data-ttu-id="45e3c-126">Tábla létrehozása a kódban</span><span class="sxs-lookup"><span data-stu-id="45e3c-126">Create a table in code</span></span>
<span data-ttu-id="45e3c-127">az Azure tábla toocreate hello csak adjon hozzá egy túl**CreateIfNotExistsAsync()**.</span><span class="sxs-lookup"><span data-stu-id="45e3c-127">toocreate hello Azure table, just add a call too**CreateIfNotExistsAsync()**.</span></span>

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="45e3c-128">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="45e3c-128">Add an entity tooa table</span></span>
<span data-ttu-id="45e3c-129">egy entitás tooa táblát hoz létre egy osztályt, amely tooadd hello az entitás tulajdonságait határozza meg.</span><span class="sxs-lookup"><span data-stu-id="45e3c-129">tooadd an entity tooa table you create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="45e3c-130">hello alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="45e3c-130">hello following code defines an entity class called **CustomerEntity** that uses hello customer's first name as hello row key and last name as hello partition key.</span></span>

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

<span data-ttu-id="45e3c-131">Entitások érintő tábla műveletet végzett hello segítségével **CloudTable** objektumot, amelyet korábban létrehozott "Hozzáférési táblák a kódban."</span><span class="sxs-lookup"><span data-stu-id="45e3c-131">Table operations involving entities are done using hello **CloudTable** object you created earlier in "Access tables in code."</span></span> <span data-ttu-id="45e3c-132">Hello **TableOperation** objektum által jelképezett hello művelet toobe történik.</span><span class="sxs-lookup"><span data-stu-id="45e3c-132">hello **TableOperation** object represents hello operation toobe done.</span></span> <span data-ttu-id="45e3c-133">a következő kód példa azt mutatja meg hogyan hello toocreate egy **CloudTable** objektum és a **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="45e3c-133">hello following code example shows how toocreate a **CloudTable** object and a **CustomerEntity** object.</span></span> <span data-ttu-id="45e3c-134">tooprepare hello műveletet, a **TableOperation** hello táblába tooinsert hello ügyfélentitást jön létre.</span><span class="sxs-lookup"><span data-stu-id="45e3c-134">tooprepare hello operation, a **TableOperation** is created tooinsert hello customer entity into hello table.</span></span> <span data-ttu-id="45e3c-135">Végezetül hello művelet CloudTable.ExecuteAsync meghívásával hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="45e3c-135">Finally, hello operation is executed by calling CloudTable.ExecuteAsync.</span></span>

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a><span data-ttu-id="45e3c-136">Entitásköteg beszúrása</span><span class="sxs-lookup"><span data-stu-id="45e3c-136">Insert a batch of entities</span></span>
<span data-ttu-id="45e3c-137">Több entitás egyetlen írási művelettel egy táblába beszúrásához.</span><span class="sxs-lookup"><span data-stu-id="45e3c-137">You can insert multiple entities into a table in a single write operation.</span></span> <span data-ttu-id="45e3c-138">hello alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), azokat beveszi tooa **TableBatchOperation** hello használó **beszúrása** metódust, és hello művelet indítása hívó CloudTable.ExecuteBatchAsync.</span><span class="sxs-lookup"><span data-stu-id="45e3c-138">hello following code example creates two entity objects ("Jeff Smith" and "Ben Smith"), adds them tooa **TableBatchOperation** object using hello **Insert** method, and then starts hello operation by calling CloudTable.ExecuteBatchAsync.</span></span>

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

## <a name="get-all-of-hello-entities-in-a-partition"></a><span data-ttu-id="45e3c-139">Egy partíció beolvasása összes hello entitások</span><span class="sxs-lookup"><span data-stu-id="45e3c-139">Get all of hello entities in a partition</span></span>
<span data-ttu-id="45e3c-140">egy tábla összes hello entitások egy partíció használjon tooquery egy **TableQuery** objektum.</span><span class="sxs-lookup"><span data-stu-id="45e3c-140">tooquery a table for all of hello entities in a partition, use a **TableQuery** object.</span></span> <span data-ttu-id="45e3c-141">hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások.</span><span class="sxs-lookup"><span data-stu-id="45e3c-141">hello following code example specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="45e3c-142">Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="45e3c-142">This example prints hello fields of each entity in hello query results toohello console.</span></span>

    // Construct hello query operation for all customer entities where PartitionKey="Smith".
    TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>().Where(TableQuery.GenerateFilterCondition("PartitionKey", QueryComparisons.Equal, "Smith"));

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

## <a name="get-a-single-entity"></a><span data-ttu-id="45e3c-143">Egyetlen entitás beolvasása</span><span class="sxs-lookup"><span data-stu-id="45e3c-143">Get a single entity</span></span>
<span data-ttu-id="45e3c-144">A lekérdezés tooget írhat egy adott entitás.</span><span class="sxs-lookup"><span data-stu-id="45e3c-144">You can write a query tooget a single, specific entity.</span></span> <span data-ttu-id="45e3c-145">hello következő kódban a **TableOperation** toospecify az ügyfél "Ben Smith" nevű objektum.</span><span class="sxs-lookup"><span data-stu-id="45e3c-145">hello following code uses a **TableOperation** object toospecify a customer named 'Ben Smith'.</span></span> <span data-ttu-id="45e3c-146">Ez a módszer értéket ad vissza, egy gyűjtemény helyett csak egyetlen entitást, és hello értéket adott vissza **Ableresult.Result** van egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="45e3c-146">This method returns just one entity, rather than a collection, and hello returned value in **TableResult.Result** is a **CustomerEntity** object.</span></span> <span data-ttu-id="45e3c-147">Hello leggyorsabb módon tooretrieve hello az egyetlen entitás partíció-és sorkulcsok megadása a lekérdezésben **tábla** szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="45e3c-147">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello **Table** service.</span></span>

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a><span data-ttu-id="45e3c-148">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="45e3c-148">Delete an entity</span></span>
<span data-ttu-id="45e3c-149">Miután megtalálta az entitás törölheti.</span><span class="sxs-lookup"><span data-stu-id="45e3c-149">You can delete an entity after you find it.</span></span> <span data-ttu-id="45e3c-150">hello alábbira megkeresi a "Ben Smith" nevű ügyfélentitást és találja, ha azt törli őket.</span><span class="sxs-lookup"><span data-stu-id="45e3c-150">hello following code looks for a customer entity named "Ben Smith" and if it finds it, it deletes it.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="45e3c-151">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="45e3c-151">Next steps</span></span>
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

