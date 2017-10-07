---
title: "a table storage és a Visual Studio (az ASP.NET Core) kapcsolódó szolgáltatások használatába aaaHow tooget |} Microsoft Docs"
description: "Hogyan tooget után indítják el, az ASP.NET Core projektben a Visual Studio Azure Table storage csatlakozás a Visual Studio használatával tooa tárfiók kapcsolódó szolgáltatások"
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
ms.openlocfilehash: 6a8fb6aa085d78a087fcd14adbc765a0d59e0308
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-started-with-azure-table-storage-and-visual-studio-connected-services"></a>Hogyan tooget el az Azure Table storage és a Visual Studio kapcsolódó szolgáltatások
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan használatának Azure Table storage a Visual Studio után készített vagy egy Azure storage-fiók ASP.NET Core projektben hivatkozott használatával hello Visual Studio **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel.

hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok. hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.

Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.

Azure Table storage használatával kapcsolatos további általános információkért lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md).

tooget elindult, először toocreate egy táblát a tárfiókban lévő. Mutat be, hogyan toocreate az Azure table-kódban. Is mutat be, hogyan tooperform egyszerű táblázat és entitás műveletek, például a hozzáadása, módosítása, olvasása, és olvasási tábla entitásokat. hello minták C nyelven íródtak\# code, és használja a hello Azure Storage ügyféloldali kódtára a .NET-hez.

**Megjegyzés:** -hello API-hívások, hogy tooAzure tároló hajtsa végre az ASP.NET Core aszinkron. Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt. az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.

## <a name="access-tables-in-code"></a>Hozzáférés táblák kódban
ASP.NET Core projektek tooaccess táblák, a következő elemek tooany C# forrásfájlok Azure table storage elérő tooinclude hello kell.

1. Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. A következő kód tooget használata hello hello a tárolási kapcsolati karakterlánc és hello Azure szolgáltatáskonfiguráció tárfiók adatait.
   
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
            CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
   
    **Megjegyzés:** -a következő minták hello az összes fenti kódot hello kód előtt hello használja.
3. Első egy **CloudTableClient** tooreference hello tábla objektumok a tárfiókban lévő objektum.  
   
        // Create hello table client.
        CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Első egy **CloudTable** objektum tooreference bizonyos táblájához és entitások.
   
        // Get a reference tooa table named "peopleTable"
        CloudTable table = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Tábla létrehozása a kódban
az Azure tábla toocreate hello csak adjon hozzá egy túl**CreateIfNotExistsAsync()**.

    // Create hello CloudTable if it does not exist
    await table.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooa táblát hoz létre egy osztályt, amely tooadd hello az entitás tulajdonságait határozza meg. hello alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és vezetékneve hello partíció kulcsként.

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

Entitások érintő tábla műveletet végzett hello segítségével **CloudTable** objektumot, amelyet korábban létrehozott "Hozzáférési táblák a kódban." Hello **TableOperation** objektum által jelképezett hello művelet toobe történik. a következő kód példa azt mutatja meg hogyan hello toocreate egy **CloudTable** objektum és a **CustomerEntity** objektum. tooprepare hello műveletet, a **TableOperation** hello táblába tooinsert hello ügyfélentitást jön létre. Végezetül hello művelet CloudTable.ExecuteAsync meghívásával hajtható végre.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);

## <a name="insert-a-batch-of-entities"></a>Entitásköteg beszúrása
Több entitás egyetlen írási művelettel egy táblába beszúrásához. hello alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), azokat beveszi tooa **TableBatchOperation** hello használó **beszúrása** metódust, és hello művelet indítása hívó CloudTable.ExecuteBatchAsync.

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

## <a name="get-all-of-hello-entities-in-a-partition"></a>Egy partíció beolvasása összes hello entitások
egy tábla összes hello entitások egy partíció használjon tooquery egy **TableQuery** objektum. hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások. Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.

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

## <a name="get-a-single-entity"></a>Egyetlen entitás beolvasása
A lekérdezés tooget írhat egy adott entitás. hello következő kódban a **TableOperation** toospecify az ügyfél "Ben Smith" nevű objektum. Ez a módszer értéket ad vissza, egy gyűjtemény helyett csak egyetlen entitást, és hello értéket adott vissza **Ableresult.Result** van egy **CustomerEntity** objektum. Hello leggyorsabb módon tooretrieve hello az egyetlen entitás partíció-és sorkulcsok megadása a lekérdezésben **tábla** szolgáltatás.

    // Create a retrieve operation that takes a customer entity.
    TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

    // Execute hello retrieve operation.
    TableResult retrievedResult = await peopleTable.ExecuteAsync(retrieveOperation);

    // Print hello phone number of hello result.
    if (retrievedResult.Result != null)
       Console.WriteLine(((CustomerEntity)retrievedResult.Result).PhoneNumber);
    else
       Console.WriteLine("hello phone number could not be retrieved.");

## <a name="delete-an-entity"></a>Entitás törlése
Miután megtalálta az entitás törölheti. hello alábbira megkeresi a "Ben Smith" nevű ügyfélentitást és találja, ha azt törli őket.

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

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [vs-storage-dotnet-tables-next-steps](../../includes/vs-storage-dotnet-tables-next-steps.md)]

