---
title: "A Table storage a Java használatával |} Microsoft Docs"
description: "Az Azure Table Storage, amely egy NoSQL-adattár, a strukturált adatok felhőben való tárolásához használható."
services: cosmos-db
documentationcenter: java
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 45145189-e67f-4ca6-b15d-43af7bfd3f97
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 7f92b1e14a514e9eda39f7ca94f63fc761dfdf41
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-from-java"></a><span data-ttu-id="78d88-103">How to use Table storage from Java (A Table Storage használata Javával)</span><span class="sxs-lookup"><span data-stu-id="78d88-103">How to use Table storage from Java</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="78d88-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="78d88-104">Overview</span></span>
<span data-ttu-id="78d88-105">Ez az útmutató bemutatja, hogyan hajthat végre a szolgáltatást az Azure Table storage szolgáltatást használó általános forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="78d88-105">This guide will show you how to perform common scenarios using the Azure Table storage service.</span></span> <span data-ttu-id="78d88-106">A mintákat a Java és -felhasználási nyelven íródtak a [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="78d88-106">The samples are written in Java and use the [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="78d88-107">Az ismertetett forgatókönyvek **létrehozása**, **listázása**, és **törlése** táblák, valamint **beszúrása**, **lekérdezése**, **módosítása**, és **törlése** entitások egy tábla.</span><span class="sxs-lookup"><span data-stu-id="78d88-107">The scenarios covered include **creating**, **listing**, and **deleting** tables, as well as **inserting**, **querying**, **modifying**, and **deleting** entities in a table.</span></span> <span data-ttu-id="78d88-108">További tudnivalókat lásd: a [további lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="78d88-108">For more information on tables, see the [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="78d88-109">Megjegyzés: Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="78d88-109">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="78d88-110">További információkért lásd: a [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="78d88-110">For more information, see the [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="78d88-111">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="78d88-111">Create a Java application</span></span>
<span data-ttu-id="78d88-112">Ez az útmutató a tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="78d88-112">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="78d88-113">Ehhez szüksége lesz a Java fejlesztői készlet (JDK) telepítése, és hozzon létre egy Azure storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="78d88-113">To do so, you will need to install the Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="78d88-114">Ha ezzel végzett, akkor győződjön meg arról, hogy a fejlesztési rendszer megfelel-e a minimális követelményeit és függőségeit, amelyek szerepelnek a [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="78d88-114">Once you have done so, you will need to verify that your development system meets the minimum requirements and dependencies which are listed in the [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="78d88-115">Ha a rendszer megfelel ezeknek a követelményeknek, akkor is kövesse letöltése és telepítése az Azure Storage szalagtárak Java ebből a rendszeren.</span><span class="sxs-lookup"><span data-stu-id="78d88-115">If your system meets those requirements, you can follow the instructions for downloading and installing the Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="78d88-116">Ezek a feladatok befejezése után lesz ebben a cikkben a példák használó Java-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="78d88-116">Once you have completed those tasks, you will be able to create a Java application which uses the examples in this article.</span></span>

## <a name="configure-your-application-to-access-table-storage"></a><span data-ttu-id="78d88-117">Állítsa be az alkalmazását, a table storage eléréséhez</span><span class="sxs-lookup"><span data-stu-id="78d88-117">Configure your application to access table storage</span></span>
<span data-ttu-id="78d88-118">Vegye fel a következő importálási utasításokat a felső részén a Java-fájlt, amelyre a Microsoft Azure storage API-kkal táblák eléréséhez használható:</span><span class="sxs-lookup"><span data-stu-id="78d88-118">Add the following import statements to the top of the Java file where you want to use Microsoft Azure storage APIs to access tables:</span></span>

```java
// Include the following imports to use table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="78d88-119">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="78d88-119">Set up an Azure storage connection string</span></span>
<span data-ttu-id="78d88-120">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc végpontok és adatok szolgáltatások eléréséhez szükséges hitelesítő adatok tárolására használ.</span><span class="sxs-lookup"><span data-stu-id="78d88-120">An Azure storage client uses a storage connection string to store endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="78d88-121">Ha egy ügyfél-alkalmazás fut, meg kell adnia a tárolási kapcsolati karakterlánc a következő formátumban a tárfiók nevével, és a tárfiók elsődleges elérési kulcs szerepel az [Azure-portálon](https://portal.azure.com) a a *AccountName* és *AccountKey* értékek.</span><span class="sxs-lookup"><span data-stu-id="78d88-121">When running in a client application, you must provide the storage connection string in the following format, using the name of your storage account and the Primary access key for the storage account listed in the [Azure portal](https://portal.azure.com) for the *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="78d88-122">Ez a példa bemutatja, hogyan deklarálhatnak ahhoz, hogy a kapcsolati karakterlánc statikus mezőben:</span><span class="sxs-lookup"><span data-stu-id="78d88-122">This example shows how you can declare a static field to hold the connection string:</span></span>

```java
// Define the connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="78d88-123">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja a konfigurációs fájlban, *ServiceConfiguration.cscfg*, és hívja érhető el a **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="78d88-123">In an application running within a role in Microsoft Azure, this string can be stored in the service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call to the **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="78d88-124">Íme egy példa a kapcsolati karakterlánc beolvasása egy **beállítás** nevű elem *StorageConnectionString* a konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="78d88-124">Here's an example of getting the connection string from a **Setting** element named *StorageConnectionString* in the service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="78d88-125">A következő minták azt feltételezik, hogy használt két módszer közül egyik beolvasni a tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="78d88-125">The following samples assume that you have used one of these two methods to get the storage connection string.</span></span>

## <a name="how-to-create-a-table"></a><span data-ttu-id="78d88-126">Útmutató: a tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="78d88-126">How to: Create a table</span></span>
<span data-ttu-id="78d88-127">A **CloudTableClient** objektum lehetővé teszi, hogy a hivatkozás objektum lekérése a táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="78d88-127">A **CloudTableClient** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="78d88-128">Az alábbi kód létrehoz egy **CloudTableClient** objektumot, és hozzon létre egy új használja **CloudTable** objektum egy tábla nevű, "személyek".</span><span class="sxs-lookup"><span data-stu-id="78d88-128">The following code creates a **CloudTableClient** object and uses it to create a new **CloudTable** object which represents a table named "people".</span></span> <span data-ttu-id="78d88-129">(Megjegyzés: további módon létrehozásához **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a a [Azure Storage ügyfél SDK-dokumentáció].)</span><span class="sxs-lookup"><span data-stu-id="78d88-129">(Note: There are additional ways to create **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in the [Azure Storage Client SDK Reference].)</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create the table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-the-tables"></a><span data-ttu-id="78d88-130">Hogyan: táblák listázása</span><span class="sxs-lookup"><span data-stu-id="78d88-130">How to: List the tables</span></span>
<span data-ttu-id="78d88-131">Ahhoz, hogy a táblák listáját, hívja az **CloudTableClient.listTables()** metódusának segítéségével lekérheti az táblanevek iterable listája.</span><span class="sxs-lookup"><span data-stu-id="78d88-131">To get a list of tables, call the **CloudTableClient.listTables()** method to retrieve an iterable list of table names.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through the collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-to-a-table"></a><span data-ttu-id="78d88-132">Hogyan: vehető fel olyan entitás egy táblához</span><span class="sxs-lookup"><span data-stu-id="78d88-132">How to: Add an entity to a table</span></span>
<span data-ttu-id="78d88-133">Entitások leképezése egy egyéni osztály végrehajtási Java objektumok **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="78d88-133">Entities map to Java objects using a custom class implementing **TableEntity**.</span></span> <span data-ttu-id="78d88-134">Kényelmi célokat szolgál a **TableServiceEntity** osztály megvalósítja **TableEntity** és képezze le a tulajdonságait a Tulajdonságok nevű elérő és beállító metódusok reflexió használatával.</span><span class="sxs-lookup"><span data-stu-id="78d88-134">For convenience, the **TableServiceEntity** class implements **TableEntity** and uses reflection to map properties to getter and setter methods named for the properties.</span></span> <span data-ttu-id="78d88-135">Egy entitás egy táblához hozzáadásához először létre kell hoznia egy osztály, amely az entitás tulajdonságait határozza meg.</span><span class="sxs-lookup"><span data-stu-id="78d88-135">To add an entity to a table, first create a class that defines the properties of your entity.</span></span> <span data-ttu-id="78d88-136">Az alábbi kód meghatároz egy entitásosztályt, amely az ügyfél keresztnevét használja, mint a sorkulcs, vezetéknevét partíciókulcsnak.</span><span class="sxs-lookup"><span data-stu-id="78d88-136">The following code defines an entity class which uses the customer's first name as the row key, and last name as the partition key.</span></span> <span data-ttu-id="78d88-137">Egy entitás partíció- és sorkulcsa együttesen azonosítja az entitást a táblán belül.</span><span class="sxs-lookup"><span data-stu-id="78d88-137">Together, an entity's partition and row key uniquely identify the entity in the table.</span></span> <span data-ttu-id="78d88-138">Az azonos partíciókulcsú entitások gyorsabban azokat a különböző partíciókulcsúak lehet lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="78d88-138">Entities with the same partition key can be queried faster than those with different partition keys.</span></span>

```java
public class CustomerEntity extends TableServiceEntity {
    public CustomerEntity(String lastName, String firstName) {
        this.partitionKey = lastName;
        this.rowKey = firstName;
    }

    public CustomerEntity() { }

    String email;
    String phoneNumber;

    public String getEmail() {
        return this.email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public String getPhoneNumber() {
        return this.phoneNumber;
    }

    public void setPhoneNumber(String phoneNumber) {
        this.phoneNumber = phoneNumber;
    }
}
```

<span data-ttu-id="78d88-139">Entitások érintő tábla működéséhez szükségesek egy **TableOperation** objektum.</span><span class="sxs-lookup"><span data-stu-id="78d88-139">Table operations involving entities require a **TableOperation** object.</span></span> <span data-ttu-id="78d88-140">Ezt az objektumot határozza meg a művelet végrehajtását entitás, amely kell végrehajtani, egy **CloudTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="78d88-140">This object defines the operation to be performed on an entity, which can be executed with a **CloudTable** object.</span></span> <span data-ttu-id="78d88-141">Az alábbi kód létrehoz egy új példányt a **CustomerEntity** osztály az egyes ügyféladatokat kell tárolni.</span><span class="sxs-lookup"><span data-stu-id="78d88-141">The following code creates a new instance of the **CustomerEntity** class with some customer data to be stored.</span></span> <span data-ttu-id="78d88-142">A következő kód hívások **TableOperation.insertOrReplace** létrehozásához egy **TableOperation** objektum entitás beszúrása egy táblát, és hozzárendeli az új **CustomerEntity** vele.</span><span class="sxs-lookup"><span data-stu-id="78d88-142">The code next calls **TableOperation.insertOrReplace** to create a **TableOperation** object to insert an entity into a table, and associates the new **CustomerEntity** with it.</span></span> <span data-ttu-id="78d88-143">Végezetül a kódja meghívja a **hajtható végre** metódust a a **CloudTable** objektum, megadva a "felhasználók" táblázat és az új **TableOperation**, amely ezután kérést küld a társzolgáltatás az új ügyfél entitás beszúrása a "felhasználók" tábla, vagy cserélje le az entitás, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="78d88-143">Finally, the code calls the **execute** method on the **CloudTable** object, specifying the "people" table and the new **TableOperation**, which then sends a request to the storage service to insert the new customer entity into the "people" table, or replace the entity if it already exists.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a><span data-ttu-id="78d88-144">Útmutató: egy teljes entitásköteget beszúrása</span><span class="sxs-lookup"><span data-stu-id="78d88-144">How to: Insert a batch of entities</span></span>
<span data-ttu-id="78d88-145">Egyetlen írási művelettel a table szolgáltatásba entitásköteget is beszúrhat.</span><span class="sxs-lookup"><span data-stu-id="78d88-145">You can insert a batch of entities to the table service in one write operation.</span></span> <span data-ttu-id="78d88-146">Az alábbi kód létrehoz egy **TableBatchOperation** objektumot, majd hozzáadja a három a beszúrási műveletek rá.</span><span class="sxs-lookup"><span data-stu-id="78d88-146">The following code creates a **TableBatchOperation** object, then adds three insert operations to it.</span></span> <span data-ttu-id="78d88-147">Minden egyes végrehajtott beszúrási művelet hozzáadott új forrásentitás-objektum létrehozása, annak értékét, és majd hívja a **beszúrása** metódust a **TableBatchOperation** objektum az entitás egy új insert művelethez társítható.</span><span class="sxs-lookup"><span data-stu-id="78d88-147">Each insert operation is added by creating a new entity object, setting its values, and then calling the **insert** method on the **TableBatchOperation** object to associate the entity with a new insert operation.</span></span> <span data-ttu-id="78d88-148">Ezután a kód hívások **hajtható végre** a a **CloudTable** objektum, adja meg a "felhasználók" tábla és a **TableBatchOperation** objektum, amely a köteg tábla műveletek küld a társzolgáltatás az egy kérelemhez.</span><span class="sxs-lookup"><span data-stu-id="78d88-148">Then the code calls **execute** on the **CloudTable** object, specifying the "people" table and the **TableBatchOperation** object, which sends the batch of table operations to the storage service in a single request.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity to add to the table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity to add to the table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity to add to the table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute the batch of operations on the "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="78d88-149">A kötegműveletekkel kapcsolatban ügyeljen a következőkre:</span><span class="sxs-lookup"><span data-stu-id="78d88-149">Some things to note on batch operations:</span></span>

* <span data-ttu-id="78d88-150">Hajtsa végre az akár 100 insert, törölheti, egyesíteni, cserélje le, Beszúrás vagy egyesítéséhez és beszúrása vagy csereműveletek bármilyen kombinációban egyetlen kötegben.</span><span class="sxs-lookup"><span data-stu-id="78d88-150">You can perform up to 100 insert, delete, merge, replace, insert or merge, and insert or replace operations in any combination in a single batch.</span></span>
* <span data-ttu-id="78d88-151">A kötegelt művelet lehet a beolvasási műveletet, ha a köteg egyetlen művelete.</span><span class="sxs-lookup"><span data-stu-id="78d88-151">A batch operation can have a retrieve operation, if it is the only operation in the batch.</span></span>
* <span data-ttu-id="78d88-152">Egy adott kötegművelet összes entitásának ugyanazzal a partíciókulccsal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="78d88-152">All entities in a single batch operation must have the same partition key.</span></span>
* <span data-ttu-id="78d88-153">A kötegelt művelet egy 4MB adattartalom korlátozódik.</span><span class="sxs-lookup"><span data-stu-id="78d88-153">A batch operation is limited to a 4MB data payload.</span></span>

## <a name="how-to-retrieve-all-entities-in-a-partition"></a><span data-ttu-id="78d88-154">Útmutató: egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="78d88-154">How to: Retrieve all entities in a partition</span></span>
<span data-ttu-id="78d88-155">Ha egy táblából egy partíció entitások, használhatja a **TableQuery**.</span><span class="sxs-lookup"><span data-stu-id="78d88-155">To query a table for entities in a partition, you can use a **TableQuery**.</span></span> <span data-ttu-id="78d88-156">Hívás **TableQuery.from** egy adott táblán, amely visszaadja a megadott típusú lekérdezés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="78d88-156">Call **TableQuery.from** to create a query on a particular table that returns a specified result type.</span></span> <span data-ttu-id="78d88-157">A következő kódot megad egy szűrőt, ahol a "Smith" a partíciókulcs az entitások.</span><span class="sxs-lookup"><span data-stu-id="78d88-157">The following code specifies a filter for entities where 'Smith' is the partition key.</span></span> <span data-ttu-id="78d88-158">**TableQuery.generateFilterCondition** van egy segédmetódust a lekérdezések-szűrők létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="78d88-158">**TableQuery.generateFilterCondition** is a helper method to create filters for queries.</span></span> <span data-ttu-id="78d88-159">Hívás **ahol** által visszaadott hivatkozás a a **TableQuery.from** módszer a szűrő alkalmazása a lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="78d88-159">Call **where** on the reference returned by the **TableQuery.from** method to apply the filter to the query.</span></span> <span data-ttu-id="78d88-160">Ha a lekérdezés végrehajtása hívja **hajtható végre** a a **CloudTable** objektumot ad vissza egy **iterátor** rendelkező a **CustomerEntity** eredménye a megadott típus.</span><span class="sxs-lookup"><span data-stu-id="78d88-160">When the query is executed with a call to **execute** on the **CloudTable** object, it returns an **Iterator** with the **CustomerEntity** result type specified.</span></span> <span data-ttu-id="78d88-161">Ezután a **iterátor** vissza egy for-each ciklus felhasználásához, az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="78d88-161">You can then use the **Iterator** returned in a for each loop to consume the results.</span></span> <span data-ttu-id="78d88-162">Ez a kód kiírja a mezőket a konzolba a lekérdezés eredményében.</span><span class="sxs-lookup"><span data-stu-id="78d88-162">This code prints the fields of each entity in the query results to the console.</span></span>

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as the partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through the results, displaying information about the entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="78d88-163">Útmutató: egy partíció entitástartományának lekérése</span><span class="sxs-lookup"><span data-stu-id="78d88-163">How to: Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="78d88-164">Ha nem szeretné egy partíció összes entitást lekérdezni, megadhat egy tartományt a szűrőben lévő összehasonlító operátorok használatával.</span><span class="sxs-lookup"><span data-stu-id="78d88-164">If you don't want to query all the entities in a partition, you can specify a range by using comparison operators in a filter.</span></span> <span data-ttu-id="78d88-165">Az alábbi kód két szűrőket, hogy a "Smith", ahol a sorkulcs (Keresztnév) legfeljebb "E" betűvel kezdődik partíció összes entitásának lekérése az ábécé egyesíti.</span><span class="sxs-lookup"><span data-stu-id="78d88-165">The following code combines two filters to get all entities in partition "Smith" where the row key (first name) starts with a letter up to 'E' in the alphabet.</span></span> <span data-ttu-id="78d88-166">Majd megjeleníti a lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="78d88-166">Then it prints the query results.</span></span> <span data-ttu-id="78d88-167">Ha használja az entitásokat, adja hozzá a kötegben táblázathoz című szakaszában talál, csak két olyan entitásra visszaadott (Ben és Denise Smith); Jeff Smith nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="78d88-167">If you use the entities added to the table in the batch insert section of this guide, only two entities are returned this time (Ben and Denise Smith); Jeff Smith is not included.</span></span>

```java
try
{
    // Define constants for filters.
    final String PARTITION_KEY = "PartitionKey";
    final String ROW_KEY = "RowKey";
    final String TIMESTAMP = "Timestamp";

    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where the partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where the row key is less than the letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine the two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as the partition key,
    // with the row key being up to the letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through the results, displaying information about the entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a><span data-ttu-id="78d88-168">Hogyan: egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="78d88-168">How to: Retrieve a single entity</span></span>
<span data-ttu-id="78d88-169">Írhat egy lekérdezést egy adott entitás lekérdezéséhez.</span><span class="sxs-lookup"><span data-stu-id="78d88-169">You can write a query to retrieve a single, specific entity.</span></span> <span data-ttu-id="78d88-170">A következő kódot a hívások **TableOperation.retrieve** partícióból kulcs vagy sor kulcs paramétereket adja meg a "Jeff Smith" létrehozása helyett egy **TableQuery** és hajtsa végre ugyanezt a szűrők használatával.</span><span class="sxs-lookup"><span data-stu-id="78d88-170">The following code calls **TableOperation.retrieve** with partition key and row key parameters to specify the customer "Jeff Smith", instead of creating a **TableQuery** and using filters to do the same thing.</span></span> <span data-ttu-id="78d88-171">Végrehajtásakor a beolvasási műveletet egy gyűjtemény helyett csak egyetlen entitást ad vissza.</span><span class="sxs-lookup"><span data-stu-id="78d88-171">When executed, the retrieve operation returns just one entity, rather than a collection.</span></span> <span data-ttu-id="78d88-172">A **getResultAsType** metódus árnyékot a hozzárendelés cél típusának eredménye egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="78d88-172">The **getResultAsType** method casts the result to the type of the assignment target, a **CustomerEntity** object.</span></span> <span data-ttu-id="78d88-173">Ha ez a típus nem kompatibilis a lekérdezéshez megadott típus, egy kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="78d88-173">If this type is not compatible with the type specified for the query, an exception will be thrown.</span></span> <span data-ttu-id="78d88-174">Null értékű ad vissza, ha nem entitás egy pontos partíció- és sorkulcsa felel meg.</span><span class="sxs-lookup"><span data-stu-id="78d88-174">A null value is returned if no entity has an exact partition and row key match.</span></span> <span data-ttu-id="78d88-175">Ha egyetlen entitást szeretne lekérdezni a Table szolgáltatásból, ennek leggyorsabb módja a partíció- és sorkulcsok megadása a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="78d88-175">Specifying both partition and row keys in a query is the fastest way to retrieve a single entity from the Table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output the entity.
    if (specificEntity != null)
    {
        System.out.println(specificEntity.getPartitionKey() +
            " " + specificEntity.getRowKey() +
            "\t" + specificEntity.getEmail() +
            "\t" + specificEntity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a><span data-ttu-id="78d88-176">Hogyan: entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="78d88-176">How to: Modify an entity</span></span>
<span data-ttu-id="78d88-177">Entitás módosítása lekéréséhez a table szolgáltatásból, módosítja a forrásentitás-objektum, és menti a módosításokat vissza az a table szolgáltatás egy áthelyezési vagy merge műveletet.</span><span class="sxs-lookup"><span data-stu-id="78d88-177">To modify an entity, retrieve it from the table service, make changes to the entity object, and save the changes back to the table service with a replace or merge operation.</span></span> <span data-ttu-id="78d88-178">A következő kód egy meglévő ügyfél telefonszámát módosítja.</span><span class="sxs-lookup"><span data-stu-id="78d88-178">The following code changes an existing customer's phone number.</span></span> <span data-ttu-id="78d88-179">Telefonhívás helyett **TableOperation.insert** beszúrása megszokott azt meghívja-e ezt a kódot **TableOperation.replace**.</span><span class="sxs-lookup"><span data-stu-id="78d88-179">Instead of calling **TableOperation.insert** like we did to insert, this code calls **TableOperation.replace**.</span></span> <span data-ttu-id="78d88-180">A **CloudTable.execute** metódus meghívja a table szolgáltatás, és az entitás helyébe, kivéve, ha egy másik alkalmazás módosított azt az időt mivel ez az alkalmazás azt lekérése.</span><span class="sxs-lookup"><span data-stu-id="78d88-180">The **CloudTable.execute** method calls the table service, and the entity is replaced, unless another application changed it in the time since this application retrieved it.</span></span> <span data-ttu-id="78d88-181">Ebben az esetben kivételt vált ki, és az entitás kell beolvasni, módosíthatók, és újra menti.</span><span class="sxs-lookup"><span data-stu-id="78d88-181">When that happens, an exception is thrown, and the entity must be retrieved, modified, and saved again.</span></span> <span data-ttu-id="78d88-182">Egyidejű hozzáférések optimista újrapróbálkozási zárolás közös az olyan elosztott tárolórendszer.</span><span class="sxs-lookup"><span data-stu-id="78d88-182">This optimistic concurrency retry pattern is common in a distributed storage system.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit the operation to the table service and get the specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation to replace the entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit the operation to the table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a><span data-ttu-id="78d88-183">Útmutató: az Entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="78d88-183">How to: Query a subset of entity properties</span></span>
<span data-ttu-id="78d88-184">A lekérdezés egy táblához pár tulajdonságok kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="78d88-184">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="78d88-185">Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében.</span><span class="sxs-lookup"><span data-stu-id="78d88-185">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="78d88-186">A lekérdezés a következő kódrészlet használja a **válasszon** metódus csak az e-mail címek entitások vissza a táblában.</span><span class="sxs-lookup"><span data-stu-id="78d88-186">The query in the following code uses the **select** method to return only the email addresses of entities in the table.</span></span> <span data-ttu-id="78d88-187">Az eredmények gyűjteménye van képezik le **karakterlánc** segítségével. egy **EntityResolver**, amelynek szerepe, hogy a kiszolgáló által visszaadott entitásokat a típuskonverziós.</span><span class="sxs-lookup"><span data-stu-id="78d88-187">The results are projected into a collection of **String** with the help of an **EntityResolver**, which does the type conversion on the entities returned from the server.</span></span> <span data-ttu-id="78d88-188">A leképezési kapcsolatos részletesebb [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="78d88-188">You can learn more about projection in [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span> <span data-ttu-id="78d88-189">Vegye figyelembe, hogy nem támogatja a helyi tárterület-emulátor, így a kód csak akkor, ha a table szolgáltatás fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="78d88-189">Note that projection is not supported on the local storage emulator, so this code runs only when using an account on the table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only the Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver to project the entity to the Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through the results, displaying the Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a><span data-ttu-id="78d88-190">Hogyan: beszúrása vagy entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="78d88-190">How to: Insert or Replace an entity</span></span>
<span data-ttu-id="78d88-191">Gyakran hozzáadandó entitást egy táblához anélkül, hogy tudnák, ha a tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="78d88-191">Often you want to add an entity to a table without knowing if it already exists in the table.</span></span> <span data-ttu-id="78d88-192">Egy beszúrása vagy lecserélése művelet lehetővé teszi egyetlen kérelem, amelyet az entitás szúrnak, ha nem létezik vagy lecserélheti a meglévőt, ellenkező esetben.</span><span class="sxs-lookup"><span data-stu-id="78d88-192">An insert-or-replace operation allows you to make a single request which will insert the entity if it does not exist or replace the existing one if it does.</span></span> <span data-ttu-id="78d88-193">Előzetes példák építve, az alábbi kód beszúrása vagy az entitás "Walter grönlandi" a felváltja.</span><span class="sxs-lookup"><span data-stu-id="78d88-193">Building on prior examples, the following code inserts or replaces the entity for "Walter Harp".</span></span> <span data-ttu-id="78d88-194">Miután létrehozta az új entitás, ez a kód meghívja a **TableOperation.insertOrReplace** metódust.</span><span class="sxs-lookup"><span data-stu-id="78d88-194">After creating a new entity, this code calls the **TableOperation.insertOrReplace** method.</span></span> <span data-ttu-id="78d88-195">Ez a kód majd meghívja a **hajtható végre** a a **CloudTable** a tábla és a Beszúrás objektumot, vagy cserélje le a tábla műveletet, mert a paraméterek.</span><span class="sxs-lookup"><span data-stu-id="78d88-195">This code then calls **execute** on the **CloudTable** object with the table and the insert or replace table operation as the parameters.</span></span> <span data-ttu-id="78d88-196">Egy entitás csak egy része frissíteni a **TableOperation.insertOrMerge** módszer is lehet használni.</span><span class="sxs-lookup"><span data-stu-id="78d88-196">To update only part of an entity, the **TableOperation.insertOrMerge** method can be used instead.</span></span> <span data-ttu-id="78d88-197">Vegye figyelembe, hogy beszúrása vagy lecserélése nem támogatott a helyi storage emulator, így a kód csak akkor, ha a table szolgáltatás fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="78d88-197">Note that insert-or-replace is not supported on the local storage emulator, so this code runs only when using an account on the table service.</span></span> <span data-ttu-id="78d88-198">További információ a beszúrása vagy lecserélése és beszúrási vagy-egyesítéses ezen tudhat [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="78d88-198">You can learn more about insert-or-replace and insert-or-merge in this [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation to add the new customer to the people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit the operation to the table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a><span data-ttu-id="78d88-199">Hogyan: entitás törlése</span><span class="sxs-lookup"><span data-stu-id="78d88-199">How to: Delete an entity</span></span>
<span data-ttu-id="78d88-200">Egy entitás azt beolvasása után egyszerűen törölheti.</span><span class="sxs-lookup"><span data-stu-id="78d88-200">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="78d88-201">Az entitás beolvasott, ha hívása **TableOperation.delete** törli az entitáshoz.</span><span class="sxs-lookup"><span data-stu-id="78d88-201">Once the entity is retrieved, call **TableOperation.delete** with the entity to delete.</span></span> <span data-ttu-id="78d88-202">Majd meghívják a **hajtható végre** a a **CloudTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="78d88-202">Then call **execute** on the **CloudTable** object.</span></span> <span data-ttu-id="78d88-203">Az alábbi kód lekérdez, majd töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="78d88-203">The following code retrieves and deletes a customer entity.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for the table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation to retrieve the entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve the entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation to delete the entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit the delete operation to the table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a><span data-ttu-id="78d88-204">Hogyan: tábla törlése</span><span class="sxs-lookup"><span data-stu-id="78d88-204">How to: Delete a table</span></span>
<span data-ttu-id="78d88-205">Végezetül a következő kódot törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="78d88-205">Finally, the following code deletes a table from a storage account.</span></span> <span data-ttu-id="78d88-206">Egy tábla már törölt újból létre kell hozni egy ideig, a törlés, körülbelül 40 másodpercig általában kevesebb, mint a következő érhető el.</span><span class="sxs-lookup"><span data-stu-id="78d88-206">A table which has been deleted will be unavailable to be recreated for a period of time following the deletion, usually less than forty seconds.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create the table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete the table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output the stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a><span data-ttu-id="78d88-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="78d88-207">Next steps</span></span>

* <span data-ttu-id="78d88-208">A [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="78d88-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="78d88-209">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="78d88-209">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="78d88-210">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="78d88-210">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="78d88-211">[Az Azure Storage REST API-n][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="78d88-211">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="78d88-212">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="78d88-212">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="78d88-213">További információ: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="78d88-213">For more information, visit [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
