---
title: "a table storage és a Visual Studio (felhőszolgáltatások) kapcsolódó szolgáltatások használatába aaaGet |} Microsoft Docs"
description: "Hogyan tooget használatának Azure Table storage egy Visual Studio felhőszolgáltatás-projektet a Visual Studio használatával tooa tárfiók kapcsolódás szolgáltatások csatlakoztatása után"
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
ms.openlocfilehash: efb16953e05764cb162cbdae4d0eab57f781682d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-azure-table-storage-and-visual-studio-connected-services-cloud-services-projects"></a>Ismerkedés az Azure table storage és a Visual Studio kapcsolódó szolgáltatások (felhőalapú szolgáltatások projektek)
[!INCLUDE [storage-try-azure-tools-tables](../../includes/storage-try-azure-tools-tables.md)]

## <a name="overview"></a>Áttekintés
Ez a cikk ismerteti, hogyan tooget el az Azure table storage a Visual Studióban létrehozott vagy egy Azure storage-fiók felhőalapú szolgáltatások projektben hivatkozott hello Visual Studio használatával után **kapcsolódó szolgáltatások hozzáadása** párbeszédpanel . Hello **kapcsolódó szolgáltatások hozzáadása** művelet hello megfelelő NuGet csomagjainak tooaccess az Azure storage telepíti a projekt és hello kapcsolati karakterláncot hozzáadja a hello tárolási fiók tooyour projekt konfigurációs fájlok.

hello Azure Table storage szolgáltatás lehetővé teszi nagy mennyiségű toostore strukturált adatok. hello szolgáltatás egy NoSQL-adattár, amely a belső és külső hello Azure-felhőbe érkező hitelesített hívásokat fogadja. Az Azure-táblák strukturált, nem relációs adatok tárolására alkalmasak.

tooget elindult, először toocreate egy táblát a tárfiókban lévő. Mutat be, hogyan toocreate az Azure table-kódban, valamint hogyan tooperform egyszerű táblázat és entitás műveletek, például a hozzáadása, módosítása, olvasása, és olvasási tábla entitások. hello minták C nyelven íródtak\# code és hello használata [Microsoft Azure Storage ügyféloldali kódtára a .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx).

**Megjegyzés:** hello API-k, hogy tooAzure tároló hívások végző aszinkron. Lásd: [aszinkron programozás az Async és Await](http://msdn.microsoft.com/library/hh191443.aspx) további információt. az alábbi kód hello azt feltételezi, hogy aszinkron programozási módszerek vannak használatban.

* Lásd: [Ismerkedés az Azure Table storage használatának .NET](storage-dotnet-how-to-use-tables.md) bővebben programozott módon kezelését.
* Lásd: [Storage-dokumentációt](https://azure.microsoft.com/documentation/services/storage/) Azure Storage kapcsolatos általános információkat.
* Lásd: [felhőalapú szolgáltatások dokumentációja](https://azure.microsoft.com/documentation/services/cloud-services/) Azure felhőszolgáltatások kapcsolatos általános információkat.
* Lásd: [ASP.NET](http://www.asp.net) ASP.NET-alkalmazások programozásával kapcsolatos további információt.

## <a name="access-tables-in-code"></a>Hozzáférés táblák kódban
cloud service projektek tooaccess táblák, a következő elemek tooany C# forrásfájlok Azure table storage elérő tooinclude hello kell.

1. Győződjön meg arról, hogy a névtér-deklarációk hello hello C# fájlban hello tetején az ezen **használatával** utasításokat.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Table;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Első egy **CloudStorageAccount** objektum, amely a tárfiók adatait jelöli. Kód tooget hello tárfiók kapcsolati karakterláncot és a tároló adatait következő hello Azure szolgáltatáskonfiguráció hello használata.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage account name>
         _AzureStorageConnectionString"));
   > [!NOTE]
   > A következő minták hello az összes fenti kódot hello kód előtt hello használja.
   > 
   > 
3. Első egy **CloudTableClient** tooreference hello tábla objektumok a tárfiókban lévő objektum.
   
         // Create hello table client.
         CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
4. Első egy **CloudTable** objektum tooreference bizonyos táblájához és entitások.
   
        // Get a reference tooa table named "peopleTable".
        CloudTable peopleTable = tableClient.GetTableReference("peopleTable");

## <a name="create-a-table-in-code"></a>Tábla létrehozása a kódban
toocreate hello Azure-tábla csak adjon hozzá egy túl**CreateIfNotExistsAsync** toohello után kap egy **CloudTable** objektum hello "Code Access táblák" című részben leírtak szerint.

    // Create hello CloudTable if it does not exist.
    await peopleTable.CreateIfNotExistsAsync();

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait. hello alábbi kód meghatároz egy entitásosztályt nevű **CustomerEntity** , hogy használ hello az ügyfél első hello sor kulcsként és hello vezetékneve hello partíció kulcsként.

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

Entitások érintő tábla műveletet végzett hello segítségével **CloudTable** objektum, amely a korábban létrehozott "Hozzáférési táblák a kódban." Hello **TableOperation** objektum által jelképezett hello művelet toobe történik. a következő kód példa azt mutatja meg hogyan hello toocreate egy **CloudTable** objektum és a **CustomerEntity** objektum. tooprepare hello műveletet, a **TableOperation** hello táblába tooinsert hello ügyfélentitást jön létre. Végezetül hello művelet meghívásával hajtható végre **CloudTable.ExecuteAsync**.

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.Email = "Walter@contoso.com";
    customer1.PhoneNumber = "425-555-0101";

    // Create hello TableOperation that inserts hello customer entity.
    TableOperation insertOperation = TableOperation.Insert(customer1);

    // Execute hello insert operation.
    await peopleTable.ExecuteAsync(insertOperation);


## <a name="insert-a-batch-of-entities"></a>Entitásköteg beszúrása
Több entitás egyetlen írási művelettel egy táblába beszúrásához. hello alábbi példakód létrehoz két entitásobjektumot ("Jeff Smith" és "Ben Smith"), azokat beveszi tooa **TableBatchOperation** objektumba hello Insert metódus, és ezután elindítja hello meghívásával művelet  **CloudTable.ExecuteBatchAsync**.

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
Miután megtalálta az entitás törölheti. hello alábbira keres egy ügyfélentitást "Ben Smith" nevű, és megtalálja, ha azt törli őket.

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

