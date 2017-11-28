---
title: a Table storage a Java aaaHow toouse |} Microsoft Docs
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
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
ms.openlocfilehash: 20d03e867219cc254da8dad37cf3cf61bca65671
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-from-java"></a><span data-ttu-id="a89d6-103">Hogyan toouse Table storage-ának Java</span><span class="sxs-lookup"><span data-stu-id="a89d6-103">How toouse Table storage from Java</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="a89d6-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a89d6-104">Overview</span></span>
<span data-ttu-id="a89d6-105">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table storage szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a89d6-105">This guide will show you how tooperform common scenarios using hello Azure Table storage service.</span></span> <span data-ttu-id="a89d6-106">hello minták Java nyelven íródtak, és használja a hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span><span class="sxs-lookup"><span data-stu-id="a89d6-106">hello samples are written in Java and use hello [Azure Storage SDK for Java][Azure Storage SDK for Java].</span></span> <span data-ttu-id="a89d6-107">hello tárgyalt forgatókönyvekben szerepel a **létrehozása**, **felsoroló**, és **törlése** táblák, valamint **beszúrása**,  **lekérdezése**, **módosítása**, és **törlése** entitások egy táblázatban.</span><span class="sxs-lookup"><span data-stu-id="a89d6-107">hello scenarios covered include **creating**, **listing**, and **deleting** tables, as well as **inserting**, **querying**, **modifying**, and **deleting** entities in a table.</span></span> <span data-ttu-id="a89d6-108">További tudnivalókat lásd: hello [további lépések](#Next-Steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="a89d6-108">For more information on tables, see hello [Next steps](#Next-Steps) section.</span></span>

<span data-ttu-id="a89d6-109">Megjegyzés: Az SDK nem elérhető a fejlesztők számára, akik Azure Storage Android-eszközökön.</span><span class="sxs-lookup"><span data-stu-id="a89d6-109">Note: An SDK is available for developers who are using Azure Storage on Android devices.</span></span> <span data-ttu-id="a89d6-110">További információkért lásd: hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span><span class="sxs-lookup"><span data-stu-id="a89d6-110">For more information, see hello [Azure Storage SDK for Android][Azure Storage SDK for Android].</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-java-application"></a><span data-ttu-id="a89d6-111">Java-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a89d6-111">Create a Java application</span></span>
<span data-ttu-id="a89d6-112">Ez az útmutató a tárolási szolgáltatásokkal, amelyek helyben, vagy a webes szerepkör vagy a feldolgozói szerepkör az Azure-ban futó belül Java-alkalmazások futtatása fogja használni.</span><span class="sxs-lookup"><span data-stu-id="a89d6-112">In this guide, you will use storage features which can be run within a Java application locally, or in code running within a web role or worker role in Azure.</span></span>

<span data-ttu-id="a89d6-113">toodo tooinstall kell tehát hello Java fejlesztői készlet (JDK), és hozzon létre egy Azure storage-fiókot az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a89d6-113">toodo so, you will need tooinstall hello Java Development Kit (JDK) and create an Azure storage account in your Azure subscription.</span></span> <span data-ttu-id="a89d6-114">Ha ezzel végzett, szüksége lesz a fejlesztői rendszerhez megfelelő hello minimális követelményeit és függőségeit, amelyek hello vannak felsorolva tooverify [Azure Storage SDK for Java] [ Azure Storage SDK for Java] GitHub tárházából.</span><span class="sxs-lookup"><span data-stu-id="a89d6-114">Once you have done so, you will need tooverify that your development system meets hello minimum requirements and dependencies which are listed in hello [Azure Storage SDK for Java][Azure Storage SDK for Java] repository on GitHub.</span></span> <span data-ttu-id="a89d6-115">Ha a rendszer megfelel ezeknek a követelményeknek, letöltése és telepítése hello Azure Storage-könyvtárakban Java ebből a rendszeren hello utasításokat kövesse.</span><span class="sxs-lookup"><span data-stu-id="a89d6-115">If your system meets those requirements, you can follow hello instructions for downloading and installing hello Azure Storage Libraries for Java on your system from that repository.</span></span> <span data-ttu-id="a89d6-116">E feladatok befejezése után fog tudni toocreate ebben a cikkben hello példák használó Java-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a89d6-116">Once you have completed those tasks, you will be able toocreate a Java application which uses hello examples in this article.</span></span>

## <a name="configure-your-application-tooaccess-table-storage"></a><span data-ttu-id="a89d6-117">Az alkalmazás tooaccess tábla tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a89d6-117">Configure your application tooaccess table storage</span></span>
<span data-ttu-id="a89d6-118">Adja hozzá a következő importálási utasítások toohello felső részén hello Java fájl toouse Microsoft Azure storage API-k tooaccess táblák hello:</span><span class="sxs-lookup"><span data-stu-id="a89d6-118">Add hello following import statements toohello top of hello Java file where you want toouse Microsoft Azure storage APIs tooaccess tables:</span></span>

```java
// Include hello following imports toouse table APIs
import com.microsoft.azure.storage.*;
import com.microsoft.azure.storage.table.*;
import com.microsoft.azure.storage.table.TableQuery.*;
```

## <a name="set-up-an-azure-storage-connection-string"></a><span data-ttu-id="a89d6-119">Egy Azure storage kapcsolati karakterlánc beállítása</span><span class="sxs-lookup"><span data-stu-id="a89d6-119">Set up an Azure storage connection string</span></span>
<span data-ttu-id="a89d6-120">Egy Azure storage-ügyfél egy tárolási kapcsolati karakterlánc toostore végpontok használ, és adatok felügyeleti szolgáltatások eléréséhez szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="a89d6-120">An Azure storage client uses a storage connection string toostore endpoints and credentials for accessing data management services.</span></span> <span data-ttu-id="a89d6-121">Ha egy ügyfél-alkalmazás fut, adja meg a hello tárolási kapcsolati karakterlánc formátuma a következő, a tárfiók nevére hello segítségével hello majd hello felsorolt hello tárfiók elsődleges elérési kulcsát hello [Azure-portálon](https://portal.azure.com)a hello *AccountName* és *AccountKey* értékeket.</span><span class="sxs-lookup"><span data-stu-id="a89d6-121">When running in a client application, you must provide hello storage connection string in hello following format, using hello name of your storage account and hello Primary access key for hello storage account listed in hello [Azure portal](https://portal.azure.com) for hello *AccountName* and *AccountKey* values.</span></span> <span data-ttu-id="a89d6-122">Ez a példa bemutatja, hogyan deklarálhatnak statikus mező toohold hello kapcsolati karakterláncot:</span><span class="sxs-lookup"><span data-stu-id="a89d6-122">This example shows how you can declare a static field toohold hello connection string:</span></span>

```java
// Define hello connection-string with your values.
public static final String storageConnectionString =
    "DefaultEndpointsProtocol=http;" +
    "AccountName=your_storage_account;" +
    "AccountKey=your_storage_account_key";
```

<span data-ttu-id="a89d6-123">Egy alkalmazás fut, a Microsoft Azure-ban a szerepkörön belüli, az ezt a karakterláncot tárolhatja hello szolgáltatás konfigurációs fájljában, *ServiceConfiguration.cscfg*, és egy hívás toohello az elérhető  **RoleEnvironment.getConfigurationSettings** metódust.</span><span class="sxs-lookup"><span data-stu-id="a89d6-123">In an application running within a role in Microsoft Azure, this string can be stored in hello service configuration file, *ServiceConfiguration.cscfg*, and can be accessed with a call toohello **RoleEnvironment.getConfigurationSettings** method.</span></span> <span data-ttu-id="a89d6-124">Íme egy példa a hello kapcsolati karakterláncának beolvasása egy **beállítás** nevű elem *StorageConnectionString* hello szolgáltatás konfigurációs fájljában:</span><span class="sxs-lookup"><span data-stu-id="a89d6-124">Here's an example of getting hello connection string from a **Setting** element named *StorageConnectionString* in hello service configuration file:</span></span>

```java
// Retrieve storage account from connection-string.
String storageConnectionString =
    RoleEnvironment.getConfigurationSettings().get("StorageConnectionString");
```

<span data-ttu-id="a89d6-125">hello következő mintákat feltételezik használt egyik alábbi két módszer tooget hello tárolási kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="a89d6-125">hello following samples assume that you have used one of these two methods tooget hello storage connection string.</span></span>

## <a name="how-to-create-a-table"></a><span data-ttu-id="a89d6-126">Útmutató: a tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="a89d6-126">How to: Create a table</span></span>
<span data-ttu-id="a89d6-127">A **CloudTableClient** objektum lehetővé teszi, hogy a hivatkozás objektum lekérése a táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="a89d6-127">A **CloudTableClient** object lets you get reference objects for tables and entities.</span></span> <span data-ttu-id="a89d6-128">hello alábbi kód létrehoz egy **CloudTableClient** objektumra, és használja ezt a toocreate új **CloudTable** objektum egy tábla nevű, "személyek".</span><span class="sxs-lookup"><span data-stu-id="a89d6-128">hello following code creates a **CloudTableClient** object and uses it toocreate a new **CloudTable** object which represents a table named "people".</span></span> <span data-ttu-id="a89d6-129">(Megjegyzés: vannak további lehetőségek toocreate **CloudStorageAccount** objektumok; további információkért lásd: **CloudStorageAccount** a hello [Azure Storage-ügyfél SDK-dokumentáció].)</span><span class="sxs-lookup"><span data-stu-id="a89d6-129">(Note: There are additional ways toocreate **CloudStorageAccount** objects; for more information, see **CloudStorageAccount** in hello [Azure Storage Client SDK Reference].)</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create hello table if it doesn't exist.
    String tableName = "people";
    CloudTable cloudTable = tableClient.getTableReference(tableName);
    cloudTable.createIfNotExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-list-hello-tables"></a><span data-ttu-id="a89d6-130">Hogyan: hello táblák listázása</span><span class="sxs-lookup"><span data-stu-id="a89d6-130">How to: List hello tables</span></span>
<span data-ttu-id="a89d6-131">tooget listáját táblákat, hívás hello **CloudTableClient.listTables()** metódus tooretrieve egy táblanevek iterable listája.</span><span class="sxs-lookup"><span data-stu-id="a89d6-131">tooget a list of tables, call hello **CloudTableClient.listTables()** method tooretrieve an iterable list of table names.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Loop through hello collection of table names.
    for (String table : tableClient.listTables())
    {
        // Output each table name.
        System.out.println(table);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-add-an-entity-tooa-table"></a><span data-ttu-id="a89d6-132">Útmutató: az entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a89d6-132">How to: Add an entity tooa table</span></span>
<span data-ttu-id="a89d6-133">Entitások leképezése egy egyéni osztály végrehajtási tooJava objektumok **TableEntity**.</span><span class="sxs-lookup"><span data-stu-id="a89d6-133">Entities map tooJava objects using a custom class implementing **TableEntity**.</span></span> <span data-ttu-id="a89d6-134">Az egyszerűség kedvéért hello **TableServiceEntity** osztály megvalósítja **TableEntity** és használ reflexiós toomap tulajdonságok toogetter és beállítására szolgáló módszerek nevű hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a89d6-134">For convenience, hello **TableServiceEntity** class implements **TableEntity** and uses reflection toomap properties toogetter and setter methods named for hello properties.</span></span> <span data-ttu-id="a89d6-135">egy entitás tooa tábla tooadd először létre kell hoznia egy osztály, amely meghatározza az entitás hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a89d6-135">tooadd an entity tooa table, first create a class that defines hello properties of your entity.</span></span> <span data-ttu-id="a89d6-136">hello alábbi kód meghatároz egy entitásosztályt, amely hello ügyfél keresztnevét használja, mint a hello sorkulcsot és Vezetéknév hello partíció kulcsként.</span><span class="sxs-lookup"><span data-stu-id="a89d6-136">hello following code defines an entity class which uses hello customer's first name as hello row key, and last name as hello partition key.</span></span> <span data-ttu-id="a89d6-137">Együtt egy entitás partíció- és sorkulcsa egyértelműen hello entitás hello táblában.</span><span class="sxs-lookup"><span data-stu-id="a89d6-137">Together, an entity's partition and row key uniquely identify hello entity in hello table.</span></span> <span data-ttu-id="a89d6-138">Ugyanazzal a partíciókulccsal gyorsabb, mint a különböző partíciókulcsúak lekérdezhetők, hello rendelkező entitások.</span><span class="sxs-lookup"><span data-stu-id="a89d6-138">Entities with hello same partition key can be queried faster than those with different partition keys.</span></span>

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

<span data-ttu-id="a89d6-139">Entitások érintő tábla működéséhez szükségesek egy **TableOperation** objektum.</span><span class="sxs-lookup"><span data-stu-id="a89d6-139">Table operations involving entities require a **TableOperation** object.</span></span> <span data-ttu-id="a89d6-140">Ez az objektum meghatározása hello művelet toobe entitás, amely kell végrehajtani, végre egy **CloudTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="a89d6-140">This object defines hello operation toobe performed on an entity, which can be executed with a **CloudTable** object.</span></span> <span data-ttu-id="a89d6-141">hello alábbi kód létrehoz egy új példányát hello **CustomerEntity** néhány tárolt felhasználói adatok toobe osztályra.</span><span class="sxs-lookup"><span data-stu-id="a89d6-141">hello following code creates a new instance of hello **CustomerEntity** class with some customer data toobe stored.</span></span> <span data-ttu-id="a89d6-142">kód következő hívások hello **TableOperation.insertOrReplace** toocreate egy **TableOperation** tooinsert entitás objektum egy táblába, és új hello társult **CustomerEntity**vele.</span><span class="sxs-lookup"><span data-stu-id="a89d6-142">hello code next calls **TableOperation.insertOrReplace** toocreate a **TableOperation** object tooinsert an entity into a table, and associates hello new **CustomerEntity** with it.</span></span> <span data-ttu-id="a89d6-143">Végezetül hello kódja meghívja hello **hajtható végre** hello metódusa **CloudTable** objektum hello "személyek" táblázat és az új hello **TableOperation**, mely majd küld egy kérelem toohello tárolási szolgáltatás tooinsert hello új ügyfélentitást hello "személyek" táblába, vagy cserélje le a hello entitás, ha már létezik.</span><span class="sxs-lookup"><span data-stu-id="a89d6-143">Finally, hello code calls hello **execute** method on hello **CloudTable** object, specifying hello "people" table and hello new **TableOperation**, which then sends a request toohello storage service tooinsert hello new customer entity into hello "people" table, or replace hello entity if it already exists.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer1 = new CustomerEntity("Harp", "Walter");
    customer1.setEmail("Walter@contoso.com");
    customer1.setPhoneNumber("425-555-0101");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer1 = TableOperation.insertOrReplace(customer1);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer1);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-a-batch-of-entities"></a><span data-ttu-id="a89d6-144">Útmutató: egy teljes entitásköteget beszúrása</span><span class="sxs-lookup"><span data-stu-id="a89d6-144">How to: Insert a batch of entities</span></span>
<span data-ttu-id="a89d6-145">Egyetlen írási művelettel egy tranzakcióköteghez entitások toohello table szolgáltatás is beszúrhat.</span><span class="sxs-lookup"><span data-stu-id="a89d6-145">You can insert a batch of entities toohello table service in one write operation.</span></span> <span data-ttu-id="a89d6-146">hello alábbi kód létrehoz egy **TableBatchOperation** objektumot, majd hozzáadja a három beszúrási műveletek tooit.</span><span class="sxs-lookup"><span data-stu-id="a89d6-146">hello following code creates a **TableBatchOperation** object, then adds three insert operations tooit.</span></span> <span data-ttu-id="a89d6-147">Minden egyes végrehajtott beszúrási művelet hozzáadott új forrásentitás-objektum létrehozása, annak értékét, és majd hívja a hello **beszúrása** hello metódusa **TableBatchOperation** tooassociate hello entitás az új objektum Helyezze be a műveletet.</span><span class="sxs-lookup"><span data-stu-id="a89d6-147">Each insert operation is added by creating a new entity object, setting its values, and then calling hello **insert** method on hello **TableBatchOperation** object tooassociate hello entity with a new insert operation.</span></span> <span data-ttu-id="a89d6-148">Ezután a kód hívások hello **hajtható végre** hello a **CloudTable** objektum hello "személyek" tábla és egyéb hello **TableBatchOperation** objektum, amely küldi hello kötegelt tábla műveletek toohello storage szolgáltatást a egyetlen kérelem.</span><span class="sxs-lookup"><span data-stu-id="a89d6-148">Then hello code calls **execute** on hello **CloudTable** object, specifying hello "people" table and hello **TableBatchOperation** object, which sends hello batch of table operations toohello storage service in a single request.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Define a batch operation.
    TableBatchOperation batchOperation = new TableBatchOperation();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a customer entity tooadd toohello table.
    CustomerEntity customer = new CustomerEntity("Smith", "Jeff");
    customer.setEmail("Jeff@contoso.com");
    customer.setPhoneNumber("425-555-0104");
    batchOperation.insertOrReplace(customer);

    // Create another customer entity tooadd toohello table.
    CustomerEntity customer2 = new CustomerEntity("Smith", "Ben");
    customer2.setEmail("Ben@contoso.com");
    customer2.setPhoneNumber("425-555-0102");
    batchOperation.insertOrReplace(customer2);

    // Create a third customer entity tooadd toohello table.
    CustomerEntity customer3 = new CustomerEntity("Smith", "Denise");
    customer3.setEmail("Denise@contoso.com");
    customer3.setPhoneNumber("425-555-0103");
    batchOperation.insertOrReplace(customer3);

    // Execute hello batch of operations on hello "people" table.
    cloudTable.execute(batchOperation);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

<span data-ttu-id="a89d6-149">Néhány dolgot toonote a kötegműveletekkel kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="a89d6-149">Some things toonote on batch operations:</span></span>

* <span data-ttu-id="a89d6-150">Too100 insert, törlés, egyesítési, csere, Beszúrás vagy egyesítés, hajtsa végre, és beszúrása vagy csereműveletek bármilyen kombinációban egyetlen kötegben.</span><span class="sxs-lookup"><span data-stu-id="a89d6-150">You can perform up too100 insert, delete, merge, replace, insert or merge, and insert or replace operations in any combination in a single batch.</span></span>
* <span data-ttu-id="a89d6-151">A kötegelt művelet lehet a beolvasási műveletet hello hello köteg egyetlen művelete esetén.</span><span class="sxs-lookup"><span data-stu-id="a89d6-151">A batch operation can have a retrieve operation, if it is hello only operation in hello batch.</span></span>
* <span data-ttu-id="a89d6-152">Hello rendelkeznie kell egy kötegművelet összes entitásának ugyanazzal a partíciókulccsal.</span><span class="sxs-lookup"><span data-stu-id="a89d6-152">All entities in a single batch operation must have hello same partition key.</span></span>
* <span data-ttu-id="a89d6-153">A kötegelt művelet korlátozott tooa 4MB adattartalom.</span><span class="sxs-lookup"><span data-stu-id="a89d6-153">A batch operation is limited tooa 4MB data payload.</span></span>

## <a name="how-to-retrieve-all-entities-in-a-partition"></a><span data-ttu-id="a89d6-154">Útmutató: egy partíció összes entitásának lekérése</span><span class="sxs-lookup"><span data-stu-id="a89d6-154">How to: Retrieve all entities in a partition</span></span>
<span data-ttu-id="a89d6-155">a partíció entitástartományának tábla tooquery, használhatja a **TableQuery**.</span><span class="sxs-lookup"><span data-stu-id="a89d6-155">tooquery a table for entities in a partition, you can use a **TableQuery**.</span></span> <span data-ttu-id="a89d6-156">Hívás **TableQuery.from** toocreate egy adott táblán, amely visszaadja a megadott típusú lekérdezést.</span><span class="sxs-lookup"><span data-stu-id="a89d6-156">Call **TableQuery.from** toocreate a query on a particular table that returns a specified result type.</span></span> <span data-ttu-id="a89d6-157">hello alábbira megad egy szűrőt entitások hello partíciós kulcs "Smith" esetén.</span><span class="sxs-lookup"><span data-stu-id="a89d6-157">hello following code specifies a filter for entities where 'Smith' is hello partition key.</span></span> <span data-ttu-id="a89d6-158">**TableQuery.generateFilterCondition** van egy segédmetódust a lekérdezések toocreate szűrők.</span><span class="sxs-lookup"><span data-stu-id="a89d6-158">**TableQuery.generateFilterCondition** is a helper method toocreate filters for queries.</span></span> <span data-ttu-id="a89d6-159">Hívás **ahol** hello által visszaadott hello hivatkozással **TableQuery.from** tooapply hello szűrő toohello lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="a89d6-159">Call **where** on hello reference returned by hello **TableQuery.from** method tooapply hello filter toohello query.</span></span> <span data-ttu-id="a89d6-160">Ha hello lekérdezést végrehajtja a rendszer a következőt hívja túl**hajtható végre** a hello **CloudTable** objektumot ad vissza egy **iterátor** a hello **CustomerEntity**eredménye a megadott típust.</span><span class="sxs-lookup"><span data-stu-id="a89d6-160">When hello query is executed with a call too**execute** on hello **CloudTable** object, it returns an **Iterator** with hello **CustomerEntity** result type specified.</span></span> <span data-ttu-id="a89d6-161">Hello segítségével **iterátor** vissza egy minden hurok tooconsume hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="a89d6-161">You can then use hello **Iterator** returned in a for each loop tooconsume hello results.</span></span> <span data-ttu-id="a89d6-162">Ez a kód kinyomtatja hello lekérdezési eredmények toohello konzolon hello mezőket.</span><span class="sxs-lookup"><span data-stu-id="a89d6-162">This code prints hello fields of each entity in hello query results toohello console.</span></span>

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

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Specify a partition query, using "Smith" as hello partition key filter.
    TableQuery<CustomerEntity> partitionQuery =
        TableQuery.from(CustomerEntity.class)
        .where(partitionFilter);

    // Loop through hello results, displaying information about hello entity.
    for (CustomerEntity entity : cloudTable.execute(partitionQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-range-of-entities-in-a-partition"></a><span data-ttu-id="a89d6-163">Útmutató: egy partíció entitástartományának lekérése</span><span class="sxs-lookup"><span data-stu-id="a89d6-163">How to: Retrieve a range of entities in a partition</span></span>
<span data-ttu-id="a89d6-164">Ha tooquery egy partíció összes hello entitások nem szeretné, megadhat egy tartományt a szűrőben lévő összehasonlító operátorok használatával.</span><span class="sxs-lookup"><span data-stu-id="a89d6-164">If you don't want tooquery all hello entities in a partition, you can specify a range by using comparison operators in a filter.</span></span> <span data-ttu-id="a89d6-165">a következő két szűrők tooget "Smith" partíció összes entitásának ahol hello sorkulcs (Keresztnév) mentése too'E betűvel kezdődik kód kombinálja hello "hello ábécé.</span><span class="sxs-lookup"><span data-stu-id="a89d6-165">hello following code combines two filters tooget all entities in partition "Smith" where hello row key (first name) starts with a letter up too'E' in hello alphabet.</span></span> <span data-ttu-id="a89d6-166">Majd jelenít hello lekérdezés eredményeit.</span><span class="sxs-lookup"><span data-stu-id="a89d6-166">Then it prints hello query results.</span></span> <span data-ttu-id="a89d6-167">Ha használ hello entitások hozzáadott toohello tábla hello kötegben című szakaszában talál, csak két olyan entitásra visszaadott (Ben és Denise Smith); Jeff Smith nincs megadva.</span><span class="sxs-lookup"><span data-stu-id="a89d6-167">If you use hello entities added toohello table in hello batch insert section of this guide, only two entities are returned this time (Ben and Denise Smith); Jeff Smith is not included.</span></span>

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

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a filter condition where hello partition key is "Smith".
    String partitionFilter = TableQuery.generateFilterCondition(
        PARTITION_KEY,
        QueryComparisons.EQUAL,
        "Smith");

    // Create a filter condition where hello row key is less than hello letter "E".
    String rowFilter = TableQuery.generateFilterCondition(
        ROW_KEY,
        QueryComparisons.LESS_THAN,
        "E");

    // Combine hello two conditions into a filter expression.
    String combinedFilter = TableQuery.combineFilters(partitionFilter,
        Operators.AND, rowFilter);

    // Specify a range query, using "Smith" as hello partition key,
    // with hello row key being up toohello letter "E".
    TableQuery<CustomerEntity> rangeQuery =
        TableQuery.from(CustomerEntity.class)
        .where(combinedFilter);

    // Loop through hello results, displaying information about hello entity
    for (CustomerEntity entity : cloudTable.execute(rangeQuery)) {
        System.out.println(entity.getPartitionKey() +
            " " + entity.getRowKey() +
            "\t" + entity.getEmail() +
            "\t" + entity.getPhoneNumber());
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-retrieve-a-single-entity"></a><span data-ttu-id="a89d6-168">Hogyan: egyetlen entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="a89d6-168">How to: Retrieve a single entity</span></span>
<span data-ttu-id="a89d6-169">A lekérdezés tooretrieve írhat egy adott entitás.</span><span class="sxs-lookup"><span data-stu-id="a89d6-169">You can write a query tooretrieve a single, specific entity.</span></span> <span data-ttu-id="a89d6-170">hello következő kódot a hívások **TableOperation.retrieve** ügyféllel partíciós kulcs és a sor paramétereket toospecify hello "Jeff Smith" létrehozása helyett egy **TableQuery** és szűrőket toodo hello használata ugyanaz.</span><span class="sxs-lookup"><span data-stu-id="a89d6-170">hello following code calls **TableOperation.retrieve** with partition key and row key parameters toospecify hello customer "Jeff Smith", instead of creating a **TableQuery** and using filters toodo hello same thing.</span></span> <span data-ttu-id="a89d6-171">Végrehajtásakor hello beolvasni egy gyűjtemény helyett művelet értéket ad vissza csak egyetlen entitást.</span><span class="sxs-lookup"><span data-stu-id="a89d6-171">When executed, hello retrieve operation returns just one entity, rather than a collection.</span></span> <span data-ttu-id="a89d6-172">Hello **getResultAsType** metódus árnyékot toohello eredménytípus hello hello hozzárendelés TARGET, egy **CustomerEntity** objektum.</span><span class="sxs-lookup"><span data-stu-id="a89d6-172">hello **getResultAsType** method casts hello result toohello type of hello assignment target, a **CustomerEntity** object.</span></span> <span data-ttu-id="a89d6-173">Ha ez a típus nem kompatibilis a hello a lekérdezéshez megadott hello típus, egy kivételt fog jelezni.</span><span class="sxs-lookup"><span data-stu-id="a89d6-173">If this type is not compatible with hello type specified for hello query, an exception will be thrown.</span></span> <span data-ttu-id="a89d6-174">Null értékű ad vissza, ha nem entitás egy pontos partíció- és sorkulcsa felel meg.</span><span class="sxs-lookup"><span data-stu-id="a89d6-174">A null value is returned if no entity has an exact partition and row key match.</span></span> <span data-ttu-id="a89d6-175">Hello leggyorsabb módon tooretrieve hello Table szolgáltatásból egy entitás partíció-és sorkulcsok megadása a lekérdezésben.</span><span class="sxs-lookup"><span data-stu-id="a89d6-175">Specifying both partition and row keys in a query is hello fastest way tooretrieve a single entity from hello Table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff"
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Output hello entity.
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
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-modify-an-entity"></a><span data-ttu-id="a89d6-176">Hogyan: entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="a89d6-176">How to: Modify an entity</span></span>
<span data-ttu-id="a89d6-177">egy entitás toomodify lekéréséhez hello table szolgáltatásból, ne módosítások toohello forrásentitás-objektum, és egy áthelyezési vagy egyesítési művelet hátsó toohello table szolgáltatás hello módosítások mentése.</span><span class="sxs-lookup"><span data-stu-id="a89d6-177">toomodify an entity, retrieve it from hello table service, make changes toohello entity object, and save hello changes back toohello table service with a replace or merge operation.</span></span> <span data-ttu-id="a89d6-178">hello alábbira vált egy meglévő ügyfél telefonszámát.</span><span class="sxs-lookup"><span data-stu-id="a89d6-178">hello following code changes an existing customer's phone number.</span></span> <span data-ttu-id="a89d6-179">Telefonhívás helyett **TableOperation.insert** tooinsert megszokott meghívja-e ezt a kódot **TableOperation.replace**.</span><span class="sxs-lookup"><span data-stu-id="a89d6-179">Instead of calling **TableOperation.insert** like we did tooinsert, this code calls **TableOperation.replace**.</span></span> <span data-ttu-id="a89d6-180">Hello **CloudTable.execute** metódushívások hello table szolgáltatás, és hello entitás helyébe, kivéve, ha egy másik alkalmazás módosított hello időben mivel ez az alkalmazás azt lekérése.</span><span class="sxs-lookup"><span data-stu-id="a89d6-180">hello **CloudTable.execute** method calls hello table service, and hello entity is replaced, unless another application changed it in hello time since this application retrieved it.</span></span> <span data-ttu-id="a89d6-181">Ebben az esetben kivételt vált ki, és hello entitás kell beolvasni, módosíthatók, és újra menti.</span><span class="sxs-lookup"><span data-stu-id="a89d6-181">When that happens, an exception is thrown, and hello entity must be retrieved, modified, and saved again.</span></span> <span data-ttu-id="a89d6-182">Egyidejű hozzáférések optimista újrapróbálkozási zárolás közös az olyan elosztott tárolórendszer.</span><span class="sxs-lookup"><span data-stu-id="a89d6-182">This optimistic concurrency retry pattern is common in a distributed storage system.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff =
        TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Submit hello operation toohello table service and get hello specific entity.
    CustomerEntity specificEntity =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Specify a new phone number.
    specificEntity.setPhoneNumber("425-555-0105");

    // Create an operation tooreplace hello entity.
    TableOperation replaceEntity = TableOperation.replace(specificEntity);

    // Submit hello operation toohello table service.
    cloudTable.execute(replaceEntity);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-query-a-subset-of-entity-properties"></a><span data-ttu-id="a89d6-183">Útmutató: az Entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="a89d6-183">How to: Query a subset of entity properties</span></span>
<span data-ttu-id="a89d6-184">A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="a89d6-184">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="a89d6-185">Ez a leképezésnek hívott technika csökkenti a sávszélesség felhasználását, és javítja a lekérdezési teljesítményt, főleg a nagy entitások esetében.</span><span class="sxs-lookup"><span data-stu-id="a89d6-185">This technique, called projection, reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="a89d6-186">hello hello kód a következő lekérdezést használ hello **válasszon** metódus tooreturn csak hello e-mail címek entitások hello táblában.</span><span class="sxs-lookup"><span data-stu-id="a89d6-186">hello query in hello following code uses hello **select** method tooreturn only hello email addresses of entities in hello table.</span></span> <span data-ttu-id="a89d6-187">hello eredmények vannak képezik le gyűjteménye **karakterlánc** hello segítségével egy **EntityResolver**, amely does hello hello entitások hello kiszolgáló által visszaadott típus átalakítást.</span><span class="sxs-lookup"><span data-stu-id="a89d6-187">hello results are projected into a collection of **String** with hello help of an **EntityResolver**, which does hello type conversion on hello entities returned from hello server.</span></span> <span data-ttu-id="a89d6-188">A leképezési kapcsolatos részletesebb [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="a89d6-188">You can learn more about projection in [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span> <span data-ttu-id="a89d6-189">Vegye figyelembe, hogy nem támogatja a hello helyi storage emulatort, így a kód csak akkor, ha a hello table szolgáltatás egy olyan fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="a89d6-189">Note that projection is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Define a projection query that retrieves only hello Email property
    TableQuery<CustomerEntity> projectionQuery =
        TableQuery.from(CustomerEntity.class)
        .select(new String[] {"Email"});

    // Define a Entity resolver tooproject hello entity toohello Email value.
    EntityResolver<String> emailResolver = new EntityResolver<String>() {
        @Override
        public String resolve(String PartitionKey, String RowKey, Date timeStamp, HashMap<String, EntityProperty> properties, String etag) {
            return properties.get("Email").getValueAsString();
        }
    };

    // Loop through hello results, displaying hello Email values.
    for (String projectedString :
        cloudTable.execute(projectionQuery, emailResolver)) {
            System.out.println(projectedString);
    }
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-insert-or-replace-an-entity"></a><span data-ttu-id="a89d6-190">Hogyan: beszúrása vagy entitás cseréje</span><span class="sxs-lookup"><span data-stu-id="a89d6-190">How to: Insert or Replace an entity</span></span>
<span data-ttu-id="a89d6-191">Gyakran érdemes tooadd egy entitás tooa tábla anélkül, hogy tudnák, ha hello tábla már létezik.</span><span class="sxs-lookup"><span data-stu-id="a89d6-191">Often you want tooadd an entity tooa table without knowing if it already exists in hello table.</span></span> <span data-ttu-id="a89d6-192">Egy beszúrása vagy lecserélése művelet lehetővé teszi egy egyetlen kérelmet, amely szúrnak hello entitás, ha nem létezik, és cserélje ki egy meglévő, ellenkező esetben hello toomake.</span><span class="sxs-lookup"><span data-stu-id="a89d6-192">An insert-or-replace operation allows you toomake a single request which will insert hello entity if it does not exist or replace hello existing one if it does.</span></span> <span data-ttu-id="a89d6-193">Előzetes példák kialakításának, hello következő kód beszúrása vagy "Walter grönlandi" hello entitás helyébe lép.</span><span class="sxs-lookup"><span data-stu-id="a89d6-193">Building on prior examples, hello following code inserts or replaces hello entity for "Walter Harp".</span></span> <span data-ttu-id="a89d6-194">Új entitás létrehozása, után ez a kód meghívja a hello **TableOperation.insertOrReplace** metódust.</span><span class="sxs-lookup"><span data-stu-id="a89d6-194">After creating a new entity, this code calls hello **TableOperation.insertOrReplace** method.</span></span> <span data-ttu-id="a89d6-195">Ez a kód majd meghívja a **hajtható végre** a hello **CloudTable** hello tábla és hello insert rendelkező objektum, vagy a tábla a csere hello paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="a89d6-195">This code then calls **execute** on hello **CloudTable** object with hello table and hello insert or replace table operation as hello parameters.</span></span> <span data-ttu-id="a89d6-196">csak egy része az entitás tooupdate hello **TableOperation.insertOrMerge** módszer is lehet használni.</span><span class="sxs-lookup"><span data-stu-id="a89d6-196">tooupdate only part of an entity, hello **TableOperation.insertOrMerge** method can be used instead.</span></span> <span data-ttu-id="a89d6-197">Vegye figyelembe, hogy beszúrása vagy lecserélése nem támogatott a hello helyi storage emulatort, így a kód csak akkor, ha a hello table szolgáltatás egy olyan fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="a89d6-197">Note that insert-or-replace is not supported on hello local storage emulator, so this code runs only when using an account on hello table service.</span></span> <span data-ttu-id="a89d6-198">További információ a beszúrása vagy lecserélése és beszúrási vagy-egyesítéses ezen tudhat [Azure táblák: Introducing Upsert és Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span><span class="sxs-lookup"><span data-stu-id="a89d6-198">You can learn more about insert-or-replace and insert-or-merge in this [Azure Tables: Introducing Upsert and Query Projection][Azure Tables: Introducing Upsert and Query Projection].</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create a new customer entity.
    CustomerEntity customer5 = new CustomerEntity("Harp", "Walter");
    customer5.setEmail("Walter@contoso.com");
    customer5.setPhoneNumber("425-555-0106");

    // Create an operation tooadd hello new customer toohello people table.
    TableOperation insertCustomer5 = TableOperation.insertOrReplace(customer5);

    // Submit hello operation toohello table service.
    cloudTable.execute(insertCustomer5);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-an-entity"></a><span data-ttu-id="a89d6-199">Hogyan: entitás törlése</span><span class="sxs-lookup"><span data-stu-id="a89d6-199">How to: Delete an entity</span></span>
<span data-ttu-id="a89d6-200">Egy entitás azt beolvasása után egyszerűen törölheti.</span><span class="sxs-lookup"><span data-stu-id="a89d6-200">You can easily delete an entity after you have retrieved it.</span></span> <span data-ttu-id="a89d6-201">Miután hello entitás beolvasott, hívja **TableOperation.delete** hello entitás toodelete együtt.</span><span class="sxs-lookup"><span data-stu-id="a89d6-201">Once hello entity is retrieved, call **TableOperation.delete** with hello entity toodelete.</span></span> <span data-ttu-id="a89d6-202">Majd meghívják a **hajtható végre** a hello **CloudTable** objektum.</span><span class="sxs-lookup"><span data-stu-id="a89d6-202">Then call **execute** on hello **CloudTable** object.</span></span> <span data-ttu-id="a89d6-203">a következő kód hello kéri le, és töröl egy ügyfélentitást.</span><span class="sxs-lookup"><span data-stu-id="a89d6-203">hello following code retrieves and deletes a customer entity.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Create a cloud table object for hello table.
    CloudTable cloudTable = tableClient.getTableReference("people");

    // Create an operation tooretrieve hello entity with partition key of "Smith" and row key of "Jeff".
    TableOperation retrieveSmithJeff = TableOperation.retrieve("Smith", "Jeff", CustomerEntity.class);

    // Retrieve hello entity with partition key of "Smith" and row key of "Jeff".
    CustomerEntity entitySmithJeff =
        cloudTable.execute(retrieveSmithJeff).getResultAsType();

    // Create an operation toodelete hello entity.
    TableOperation deleteSmithJeff = TableOperation.delete(entitySmithJeff);

    // Submit hello delete operation toohello table service.
    cloudTable.execute(deleteSmithJeff);
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```

## <a name="how-to-delete-a-table"></a><span data-ttu-id="a89d6-204">Hogyan: tábla törlése</span><span class="sxs-lookup"><span data-stu-id="a89d6-204">How to: Delete a table</span></span>
<span data-ttu-id="a89d6-205">Végezetül hello alábbira törölhető egy tábla a tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="a89d6-205">Finally, hello following code deletes a table from a storage account.</span></span> <span data-ttu-id="a89d6-206">Egy tábla már törölt újra létrehozza a következő hello törlésre, általában kevesebb, mint körülbelül 40 másodpercig egy ideig nem érhető el toobe lesz.</span><span class="sxs-lookup"><span data-stu-id="a89d6-206">A table which has been deleted will be unavailable toobe recreated for a period of time following hello deletion, usually less than forty seconds.</span></span>

```java
try
{
    // Retrieve storage account from connection-string.
    CloudStorageAccount storageAccount =
        CloudStorageAccount.parse(storageConnectionString);

    // Create hello table client.
    CloudTableClient tableClient = storageAccount.createCloudTableClient();

    // Delete hello table and all its data if it exists.
    CloudTable cloudTable = tableClient.getTableReference("people");
    cloudTable.deleteIfExists();
}
catch (Exception e)
{
    // Output hello stack trace.
    e.printStackTrace();
}
```
[!INCLUDE [storage-check-out-samples-java](../../includes/storage-check-out-samples-java.md)]

## <a name="next-steps"></a><span data-ttu-id="a89d6-207">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a89d6-207">Next steps</span></span>

* <span data-ttu-id="a89d6-208">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="a89d6-208">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="a89d6-209">[Az Azure Storage Java SDK][Azure Storage SDK for Java]</span><span class="sxs-lookup"><span data-stu-id="a89d6-209">[Azure Storage SDK for Java][Azure Storage SDK for Java]</span></span>
* <span data-ttu-id="a89d6-210">[Az Azure Storage ügyfél SDK-dokumentáció][Azure Storage-ügyfél SDK-dokumentáció]</span><span class="sxs-lookup"><span data-stu-id="a89d6-210">[Azure Storage Client SDK Reference][Azure Storage Client SDK Reference]</span></span>
* <span data-ttu-id="a89d6-211">[Az Azure Storage REST API-n][Azure Storage REST API]</span><span class="sxs-lookup"><span data-stu-id="a89d6-211">[Azure Storage REST API][Azure Storage REST API]</span></span>
* <span data-ttu-id="a89d6-212">[Az Azure Storage csapat blogja][Azure Storage Team Blog]</span><span class="sxs-lookup"><span data-stu-id="a89d6-212">[Azure Storage Team Blog][Azure Storage Team Blog]</span></span>

<span data-ttu-id="a89d6-213">További információ: [Azure Java-fejlesztőknek](/java/azure).</span><span class="sxs-lookup"><span data-stu-id="a89d6-213">For more information, visit [Azure for Java developers](/java/azure).</span></span>

[Azure SDK for Java]: http://go.microsoft.com/fwlink/?LinkID=525671
[Azure Storage SDK for Java]: https://github.com/azure/azure-storage-java
[Azure Storage SDK for Android]: https://github.com/azure/azure-storage-android
[Azure Storage-ügyfél SDK-dokumentáció]: http://dl.windowsazure.com/storage/javadoc/
[Azure Storage REST API]: https://msdn.microsoft.com/library/azure/dd179355.aspx
[Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
[Azure Tables: Introducing Upsert and Query Projection]: http://blogs.msdn.com/b/windowsazurestorage/archive/2011/09/15/windows-azure-tables-introducing-upsert-and-query-projection.aspx
