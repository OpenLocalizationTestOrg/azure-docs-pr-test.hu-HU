---
title: "aaaGet Azure Table storage használatának .NET használatába |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
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
ms.openlocfilehash: a3e9a4c6f6fd5e724535b86a3f99cd4c161de6de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-table-storage-using-net"></a>Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

Azure Table storage egy olyan szolgáltatás, amely strukturált NoSQL hello felhőben biztosít egy kulcsattribútum adattároló séma nélküli kialakítás tárolja. Mivel a Table storage séma nélküli, akkor egyszerűen tooadapt alkalmazás igényeinek változásával kell hello az adatokat. Hozzáférés tooTable tárolási adatok gyors és költséghatékony, számos különböző típusú alkalmazásokat, és általában kevesebb költséggel jár, mint a hagyományos SQL hasonló adatmennyiséggel.

Webes alkalmazásokhoz, címtárakat, eszközadatokat és egyéb metaadatokat a szolgáltatásnak szüksége van a Table storage toostore rugalmas adatkészleteket például a felhasználói adatok is használhatja. Korlátlan számú entitást tárolhat egy táblát, és a storage-fiókok tartalmazhat korlátlan számú táblát toohello kapacitásán belül hello storage-fiók mentése.

### <a name="about-this-tutorial"></a>Az oktatóanyag ismertetése
Az oktatóanyag bemutatja, hogyan toouse hello [Azure Storage ügyféloldali kódtára a .NET](https://www.nuget.org/packages/WindowsAzure.Storage/) az egyes Azure Table storage szabhatják. A forgatókönyveket a táblák létrehozásának és törlésének, valamint a táblaadatok beillesztésének, frissítésének, törlésének és lekérdezésének a C# programozási nyelvben írt példáin keresztül mutatjuk be.

## <a name="prerequisites"></a>Előfeltételek

Meg kell hello toocomplete követően ez az oktatóanyag sikeresen:

* [Microsoft Visual Studio](https://www.visualstudio.com/downloads/)
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára](https://www.nuget.org/packages/WindowsAzure.Storage/)
* [Azure Configuration Manager a .NET-hez](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
* [Azure Storage-fiók](../storage/common/storage-create-storage-account.md#create-a-storage-account)

[!INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>További példák
További példák a Table Storage használatára: [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel). Töltse le a mintaalkalmazást hello és futtatáshoz, vagy keresse meg a hello kódja a Githubon.

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[!INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-using-directives"></a>Hozzáadás irányelvekkel
Adja hozzá a következő hello **használatával** irányelvek toohello felső részén hello `Program.cs` fájlt:

```csharp
using Microsoft.Azure; // Namespace for CloudConfigurationManager
using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
using Microsoft.WindowsAzure.Storage.Table; // Namespace for Table storage types
```

### <a name="parse-hello-connection-string"></a>Hello kapcsolati karakterlánc elemzése
[!INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-hello-table-service-client"></a>Hello Table szolgáltatásügyfél létrehozása
Hello [CloudTableClient] [ dotnet_CloudTableClient] osztály lehetővé teszi a Table storage-ban tárolt tooretrieve táblákat és entitásokat. Egyirányú toocreate hello Table szolgáltatásügyfél itt található:

```csharp
// Create hello table client.
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();
```

Most már készen áll a toowrite kódot, amely adatokat olvas és ír tooTable adattárolás áll.

## <a name="create-a-table"></a>Tábla létrehozása
A példa bemutatja, hogyan toocreate egy táblát, ha még nem létezik:

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

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása
Entitások tooC # objektumok leképezése egy származtatott egyéni osztály használatával [TableEntity][dotnet_TableEntity]. egy entitás tooa tábla tooadd hozzon létre egy osztályt, amely meghatározza az entitás hello tulajdonságait. hello alábbi kód meghatároz egy entitásosztályt, amely hello sorkulcsnak és a Vezetéknév hello partíció kulcsként hello ügyfél keresztnevét használja. Együtt egy entitás partíció- és sorkulcsa egyedi azonosítása céljából hello táblában. Ugyanazzal a partíciókulccsal különböző rendelkező entitások gyorsabban lekérdezhetők, hello rendelkező entitások partícióazonosító kulcsok, de az eltérő partíciókulcsok használata a párhuzamos műveletek nagyobb méretezhetőségét teszi lehetővé. Entitások toobe táblákban tárolt kell lennie, például származó hello támogatott típusú [TableEntity] [ dotnet_TableEntity] osztály. Azt szeretné, hogy a tábla toostore Entitástulajdonságok kell hello típus nyilvános tulajdonságait, és támogatja az első és az értékek beállítás. Az entitástípusnak emellett elérhetővé *kell* tennie egy paraméter nélküli konstruktort is.

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

Tábla, például az entitások műveleteket keresztül hello [CloudTable] [ dotnet_CloudTable] hello "Tábla létrehozása" szakaszban korábban létrehozott objektum. hello végre műveletet toobe által képviselt egy [TableOperation] [ dotnet_TableOperation] objektum. hello alábbi példakód bemutatja hello hello létrehozása [CloudTable] [ dotnet_CloudTable] objektumot, majd egy **CustomerEntity** objektum. tooprepare hello műveletet, a [TableOperation] [ dotnet_TableOperation] objektum létrehozása tooinsert hello ügyfélentitást hello táblába. Végezetül hello művelet meghívásával hajtható végre [CloudTable][dotnet_CloudTable].[ Végrehajtás][dotnet_CloudTable_Execute].

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

## <a name="insert-a-batch-of-entities"></a>Entitásköteg beszúrása
Egyetlen írási művelettel egy teljes entitásköteget is beszúrhat egy táblába. Néhány további megjegyzés a kötegműveletekkel kapcsolatban:

* Frissítések, törlés és Beszúrás végezheti el a hello megegyezik egy kötegelt művelet.
* Egy kötegművelet mentése too100 entitások tartalmazhatnak.
* Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.
* Bár ez a lekérdezés kötegelt műveletként lehetséges tooperform, hello hello köteg egyetlen művelete kell lennie.

hello alábbi példakód létrehoz két entitásobjektumot, és hozzáadja minden egyes túl[TableBatchOperation] [ dotnet_TableBatchOperation] hello segítségével [beszúrása] [ dotnet_TableBatchOperation_Insert] módszer. Ezt követően [CloudTable][dotnet_CloudTable].[ ExecuteBatch] [ dotnet_CloudTable_ExecuteBatch] tooexecute hello művelet neve.

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

## <a name="retrieve-all-entities-in-a-partition"></a>Egy partíció összes entitásának lekérése
egy tábla használja egy partíció összes entitását tooquery egy [TableQuery] [ dotnet_TableQuery] objektum. hello alábbi példakód megad egy szűrőt, ahol a "Smith" hello partíciókulcs az entitások. Ez a példa kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.

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

## <a name="retrieve-a-range-of-entities-in-a-partition"></a>Partíció entitástartományának lekérése
Egy partíció összes entitásának tooquery nem szeretné, ha egy tartományt is megadhat hello partíció kulcs szűrő egy sor szűrőjének kombinálásával. hello alábbi példakód két szűrő tooget összes entitásának a 'Smith', amelyen hello sorkulcs (Keresztnév) hello ábécé előtt "E" betűvel kezdődik, majd kinyomtatja hello lekérdezési eredmények partíció.

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

## <a name="retrieve-a-single-entity"></a>Egyetlen entitás lekérdezése
A lekérdezés tooretrieve írhat egy adott entitás. hello következő kódban [TableOperation] [ dotnet_TableOperation] toospecify hello ügyfél "Ben Smith". Ez a módszer a gyűjtemény helyett csak egyetlen entitást adja vissza, és hello értéket adott vissza [TableResult][dotnet_TableResult].[ Eredmény] [ dotnet_TableResult_Result] van egy **CustomerEntity** objektum. Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.

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

## <a name="replace-an-entity"></a>Entitás cseréje
tooupdate egy entitás lekéréséhez hello Table szolgáltatásból, módosítsa a hello forrásentitás-objektum, és majd hello módosítások mentése toohello Table szolgáltatás biztonsági. hello alábbira vált egy meglévő ügyfél telefonszámát. Az [Insert][dotnet_TableOperation_Insert] parancs hívása helyett a kód a [Replace][dotnet_TableOperation_Replace] parancsot használja. [Cserélje le] [ dotnet_TableOperation_Replace] okok hello entitás toobe hello kiszolgálón teljesen lecseréli, kivéve, ha a lekérdezés, amely esetben hello sikertelen lesz hello entitás hello kiszolgálón megváltozott. Ez a hiba az alkalmazás ne írhasson felül véletlenül olyan módosítás történt közötti hello lekérését, és frissítse az alkalmazás egy másik összetevő tooprevent. hello Ez a hiba megfelelő kezeléséhez tooretrieve hello entitás újra, hajtsa végre a módosításokat (Ha még érvényesek), és ezután hajtson végre egy újabb [cserélje le] [ dotnet_TableOperation_Replace] műveletet. hello a következő szakasz bemutatja, hogyan toooverride ezt a viselkedést.

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

## <a name="insert-or-replace-an-entity"></a>Entitás beszúrása vagy lecserélése
[Cserélje le] [ dotnet_TableOperation_Replace] műveletek sikertelenek lesznek, ha hello entitás hello kiszolgálóról lekérdezés óta módosult. Ezenkívül be kell olvasni hello entitás hello kiszolgálóról először ahhoz, hogy hello [cserélje le] [ dotnet_TableOperation_Replace] művelet toobe sikeres. Egyes esetekben azonban nem tudja Ha hello entitás létezik-e hello kiszolgálón, és hello a benne tárolt aktuális értékek irrelevánsak. A frissítés mindent felülír. tooaccomplish, szeretné használni egy [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] műveletet. Ez a művelet beszúrja hello entitás, ha nem létezik, vagy ha igen, mikor hello utolsó frissítés történt függetlenül lecseréli.

Az alábbi kódpéldát hello egy ügyfélentitást "János János" a létrehozott és hello "személyek" táblába. A következő használjuk hello [InsertOrReplace] [ dotnet_TableOperation_InsertOrReplace] művelet toosave hello rendelkező entitás azonos partíciós kulcs (János) és a sor kulcs (János) toohello kiszolgáló, egy másik érték megadásával hello PhoneNumber most tulajdonság. Mivel az [InsertOrReplace][dotnet_TableOperation_InsertOrReplace] műveletet használjuk, minden tulajdonság értéke lecserélődik. Azonban ha egy "János János" entitás már hello tábla nem létezik, akkor volna beszúrását.

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

## <a name="query-a-subset-of-entity-properties"></a>Az entitástulajdonságok egy részének lekérdezése
A lekérdezés pár tulajdonságok beolvasható egy entitás helyett minden hello entitás tulajdonságai. Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében. a következő kód hello hello lekérdezést csak hello e-mail címek entitások hello tábla adja vissza. Ez a [DynamicTableEntity][dotnet_DynamicTableEntity] és az [EntityResolver][dotnet_EntityResolver] lekérdezésekkel hajtható végre. A hello leképezése kapcsolatos részletesebb [Introducing Upsert és Query Projection blogbejegyzés][blog_post_upsert]. Leképezés nem támogatja hello storage emulatort, így a kód csak akkor, ha a fiók hello Table szolgáltatás használ.

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

## <a name="delete-an-entity"></a>Entitás törlése
Egyszerűen törölheti entitás hello segítségével beolvasása után egy entitás frissítésénél bemutatott minta. a következő kód hello kéri le, és töröl egy ügyfélentitást.

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

## <a name="delete-a-table"></a>Tábla törlése
Végezetül alábbi kódpéldát hello törölhető egy tábla a tárfiókból. A törölt tábla nem érhető el toobe egy adott időn belül hello törlését követően újra létre lesz.

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

## <a name="retrieve-entities-in-pages-asynchronously"></a>Oldalak entitásainak aszinkron lekérése
Ha sok entitást olvas, és a kívánt tooprocess/megjelenítendő entitások lehívás entitás összes tooreturn, akkor kérje le az entitásokat szegmentált lekérdezéssel helyett. Ez a példa bemutatja, hogyan tooreturn eredményezi, hogy közben nem blokkolja a végrehajtási hello Async-Await mintázat használatával lapok nagy számú eredmény tooreturn vár. További információk az Async-Await mintázat a .NET hello, lásd: [aszinkron programozás az Async és Await (C# és Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx).

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

## <a name="next-steps"></a>Következő lépések
Most, hogy megismerte a Table storage alapjait hello, hajtsa végre az összetettebb tárolási feladatok elvégzéséről kapcsolatos alábbi hivatkozások toolearn:

* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.
* További Table Storage-példákat a [Getting Started with Azure Table Storage in .NET](https://azure.microsoft.com/documentation/samples/storage-table-dotnet-getting-started/) (Az Azure Table Storage használatának első lépései a .NET-keretrendszerrel) című cikkben tekinthet meg.
* Nézet hello tábla szolgáltatás elérhető API-kat vonatkozó dokumentációját:
* [Az Azure Storage .NET-hez készült ügyféloldali kódtára – referencia](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
* [REST API – referencia](http://msdn.microsoft.com/library/azure/dd179355)
* Ismerje meg, hogyan toosimplify hello kódot ír hello segítségével az Azure Storage toowork [Azure WebJobs SDK](../app-service-web/websites-dotnet-webjobs-sdk-get-started.md)
* Azure-ban való adattárolás további lehetőségeiről további szolgáltatás útmutatók toolearn megtekintése.
* [Az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md) toostore strukturálatlan adatok.
* [.NET (C#) használatával kapcsolódnak a tooSQL adatbázis](../sql-database/sql-database-develop-dotnet-simple.md) toostore relációs adatok.

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
