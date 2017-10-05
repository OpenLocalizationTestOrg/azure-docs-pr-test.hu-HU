---
title: "Ruby az Azure Table Storage használata |} Microsoft Docs"
description: "Az Azure Table Storage, amely egy NoSQL-adattár, a strukturált adatok felhőben való tárolásához használható."
services: cosmos-db
documentationcenter: ruby
author: mimig1
manager: jhubbard
editor: 
ms.assetid: 047cd9ff-17d3-4c15-9284-1b5cc61a3224
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: ruby
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: 372bc89f75ad4730f0defbf9d6f9f041ae5ce1bf
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-azure-table-storage-from-ruby"></a><span data-ttu-id="e2f8a-103">Ruby az Azure Table Storage használata</span><span class="sxs-lookup"><span data-stu-id="e2f8a-103">How to use Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="e2f8a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e2f8a-104">Overview</span></span>
<span data-ttu-id="e2f8a-105">Ez az útmutató bemutatja, hogyan hajthat végre a gyakori forgatókönyvek az Azure Table szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-105">This guide shows you how to perform common scenarios using the Azure Table service.</span></span> <span data-ttu-id="e2f8a-106">A minták íródtak, a Ruby API használatával.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-106">The samples are written using the Ruby API.</span></span> <span data-ttu-id="e2f8a-107">Az ismertetett forgatókönyvek **létrehozása egy tábla törlésével, és a tábla az entitások lekérdezése**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-107">The scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="e2f8a-108">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2f8a-108">Create a Ruby application</span></span>
<span data-ttu-id="e2f8a-109">További tájékoztatást szeretne Ruby-alkalmazás létrehozása, olvassa el [Ruby sínek webalkalmazás egy Azure virtuális gépen](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="e2f8a-109">For instructions how to create a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-to-access-storage"></a><span data-ttu-id="e2f8a-110">Állítsa be az alkalmazását tároló elérésére</span><span class="sxs-lookup"><span data-stu-id="e2f8a-110">Configure your application to access Storage</span></span>
<span data-ttu-id="e2f8a-111">Azure Storage használatához szüksége töltse le és használja a Ruby azure csomagot tartalmaz, a felhasználók kényelme érdekében szalagtár szerepel, amely a többi tárolási szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-111">To use Azure Storage, you need to download and use the Ruby azure package which includes a set of convenience libraries that communicate with the Storage REST services.</span></span>

### <a name="use-rubygems-to-obtain-the-package"></a><span data-ttu-id="e2f8a-112">RubyGems használja a csomag beszerzése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-112">Use RubyGems to obtain the package</span></span>
1. <span data-ttu-id="e2f8a-113">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="e2f8a-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="e2f8a-114">Típus **gem telepítése azure** a gem és függőségeinek telepítésére a parancsablakban.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-114">Type **gem install azure** in the command window to install the gem and dependencies.</span></span>

### <a name="import-the-package"></a><span data-ttu-id="e2f8a-115">A csomag importálása</span><span class="sxs-lookup"><span data-stu-id="e2f8a-115">Import the package</span></span>
<span data-ttu-id="e2f8a-116">Kedvenc szövegszerkesztőjével használja, a Ruby, ahol tárolására használni kívánt fájl elejéhez adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-116">Use your favorite text editor, add the following to the top of the Ruby file where you intend to use Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="e2f8a-117">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="e2f8a-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="e2f8a-118">Az azure-moduljának a környezeti változókat olvassák **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_hozzáférés\_kulcs** az Azure Storage-fiók eléréséhez szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-118">The azure module will read the environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required to connect to your Azure Storage account.</span></span> <span data-ttu-id="e2f8a-119">Ha ezek a környezeti változók nem, meg kell adnia a fiók használata előtt **Azure::TableService** az alábbi kódra:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-119">If these environment variables are not set, you must specify the account information before using **Azure::TableService** with the following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="e2f8a-120">Ezek az értékek beolvasása klasszikus vagy erőforrás-kezelő storage-fiókot az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-120">To obtain these values from a classic or Resource Manager storage account in the Azure portal:</span></span>

1. <span data-ttu-id="e2f8a-121">Jelentkezzen be az [Azure portálra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="e2f8a-121">Log in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="e2f8a-122">Nyissa meg a használni kívánt tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-122">Navigate to the storage account you want to use.</span></span>
3. <span data-ttu-id="e2f8a-123">Kattintson a beállítások panelen kattintson a jobb **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-123">In the Settings blade on the right, click **Access Keys**.</span></span>
4. <span data-ttu-id="e2f8a-124">A elérési kulcsok paneljén megjelenő láthatja, a hozzáférési kulcs 1 és 2 elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-124">In the Access keys blade that appears, you'll see the access key 1 and access key 2.</span></span> <span data-ttu-id="e2f8a-125">Ezek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-125">You can use either of these.</span></span>
5. <span data-ttu-id="e2f8a-126">A Másolás ikonra a kulcs másolása a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-126">Click the copy icon to copy the key to the clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="e2f8a-127">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="e2f8a-127">Create a table</span></span>
<span data-ttu-id="e2f8a-128">A **Azure::TableService** objektum lehetővé teszi, hogy a táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-128">The **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="e2f8a-129">A tábla létrehozásához használja a **létrehozása\_table()** metódust.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-129">To create a table, use the **create\_table()** method.</span></span> <span data-ttu-id="e2f8a-130">Az alábbi példa létrehoz egy táblát, vagy kiírja a hiba, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-130">The following example creates a table or prints the error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="e2f8a-131">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="e2f8a-131">Add an entity to a table</span></span>
<span data-ttu-id="e2f8a-132">Egy entitás hozzáadásához először létre kell hoznia egy ujjlenyomat-objektumot, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-132">To add an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="e2f8a-133">Vegye figyelembe, hogy minden entitás meg kell adnia egy **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-133">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="e2f8a-134">Ezek az entitások egyedi azonosítói, és sokkal gyorsabb, mint a többi tulajdonság lekérdezett érték.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-134">These are the unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="e2f8a-135">Használja az Azure Storage **PartitionKey** automatikusan terjesztené a táblaentitásokat számos tárolási csomópont feladatait.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-135">Azure Storage uses **PartitionKey** to automatically distribute the table's entities over many storage nodes.</span></span> <span data-ttu-id="e2f8a-136">Entitások azonos **PartitionKey** ugyanazon a csomóponton találhatók.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-136">Entities with the same **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="e2f8a-137">A **RowKey** tartozik, a partíción belül entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-137">The **RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="e2f8a-138">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-138">Update an entity</span></span>
<span data-ttu-id="e2f8a-139">Több módszer érhető el frissíteni a meglévő entitás áll rendelkezésre:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-139">There are multiple methods available to update an existing entity:</span></span>

* <span data-ttu-id="e2f8a-140">**frissítés\_entity():** cserélje ki egy meglévő entitás frissítése.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-140">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="e2f8a-141">**Egyesítési\_entity():** meglévő entitás frissíti a meglévő entitás új tulajdonságértékek egyesíti.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-141">**merge\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span>
* <span data-ttu-id="e2f8a-142">**Helyezze be\_vagy\_egyesítési\_entity():** cserélje ki egy meglévő entitás frissíti.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-142">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="e2f8a-143">Ha nem entitás létezik, egy új szúrja be:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-143">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="e2f8a-144">**Helyezze be\_vagy\_cserélje le\_entity():** meglévő entitás frissíti a meglévő entitás új tulajdonságértékek egyesíti.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-144">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into the existing entity.</span></span> <span data-ttu-id="e2f8a-145">Ha nem entitás létezik, a program beszúrja egy újat.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-145">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="e2f8a-146">A következő példa bemutatja, hogy egy entitás használatával frissítése **frissítése\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-146">The following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="e2f8a-147">A **frissítése\_entity()** és **egyesítési\_entity()**, ha a frissítendő entitás nem létezik, akkor a frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-147">With **update\_entity()** and **merge\_entity()**, if the entity that you are updating doesn't exist then the update operation will fail.</span></span> <span data-ttu-id="e2f8a-148">Ezért ha egy entitás, függetlenül attól, hogy létezik-e tárolni, Ehelyett használjon **beszúrása\_vagy\_cserélje le\_entity()** vagy **beszúrása\_vagy\_egyesítési\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-148">Therefore if you wish to store an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="e2f8a-149">Az entitások csoportok használata</span><span class="sxs-lookup"><span data-stu-id="e2f8a-149">Work with groups of entities</span></span>
<span data-ttu-id="e2f8a-150">Egyes esetekben érdemes elküldeni a több műveletei együtt egy kötegelt atomi feldolgozása a kiszolgáló biztosításához.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-150">Sometimes it makes sense to submit multiple operations together in a batch to ensure atomic processing by the server.</span></span> <span data-ttu-id="e2f8a-151">Hajthatja végre, hogy, először létre kell hoznia egy **kötegelt** objektumot, majd a **hajtható végre\_batch()** metódusa **TableService**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-151">To accomplish that, you first create a **Batch** object and then use the **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="e2f8a-152">A következő példa bemutatja, hogy két olyan entitásra RowKey 2 és 3 kötegben elküldése.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-152">The following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="e2f8a-153">Figyelje meg, hogy csak az azonos PartitionKey rendelkező entitások működik.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-153">Notice that it only works for entities with the same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="e2f8a-154">Egy entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-154">Query for an entity</span></span>
<span data-ttu-id="e2f8a-155">Egy tábla egy entitás lekérdezéséhez használja a **beolvasása\_entity()** metódus úgy, hogy a táblázat neve **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-155">To query an entity in a table, use the **get\_entity()** method, by passing the table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="e2f8a-156">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-156">Query a set of entities</span></span>
<span data-ttu-id="e2f8a-157">Az egy tábla entitások készletének lekérdezéséhez létrehozni lekérdezés kivonat objektumot, és használja a **lekérdezés\_entities()** metódust.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-157">To query a set of entities in a table, create a query hash object and use the **query\_entities()** method.</span></span> <span data-ttu-id="e2f8a-158">A következő példa bemutatja, hogy ugyanarra a entitások első **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="e2f8a-158">The following example demonstrates getting all the entities with the same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="e2f8a-159">Ha az eredményhalmaz van visszaállításához a folytatási kód adja vissza, amelyek segítségével beolvasni a következő lapjain egyetlen lekérdezés túl nagy.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-159">If the result set is too large for a single query to return, a continuation token will be returned which you can use to retrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="e2f8a-160">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-160">Query a subset of entity properties</span></span>
<span data-ttu-id="e2f8a-161">A lekérdezés egy táblához pár tulajdonságok kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-161">A query to a table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="e2f8a-162">Ez a módszer, "leképezés" nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-162">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="e2f8a-163">A select záradékban használható, és adja át a tulajdonságok az ügyfél kerüljön szeretné a nevét.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-163">Use the select clause and pass the names of the properties you would like to bring over to the client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="e2f8a-164">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-164">Delete an entity</span></span>
<span data-ttu-id="e2f8a-165">Egy entitás törléséhez használja a **törlése\_entity()** metódust.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-165">To delete an entity, use the **delete\_entity()** method.</span></span> <span data-ttu-id="e2f8a-166">Az entitás-, a PartitionKey és az entitás RowKey tartalmazó tábla nevében továbbítani kell.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-166">You need to pass in the name of the table which contains the entity, the PartitionKey and RowKey of the entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="e2f8a-167">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="e2f8a-167">Delete a table</span></span>
<span data-ttu-id="e2f8a-168">Egy tábla törléséhez használja a **törlése\_table()** metódus és pass a törölni kívánt tábla nevében.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-168">To delete a table, use the **delete\_table()** method and pass in the name of the table you want to delete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="e2f8a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e2f8a-169">Next steps</span></span>

* <span data-ttu-id="e2f8a-170">A [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, önálló alkalmazás, amelynek segítségével vizuálisan dolgozhat Azure Storage-adatokkal Windows, macOS és Linux rendszereken.</span><span class="sxs-lookup"><span data-stu-id="e2f8a-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you to work visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="e2f8a-171">[Rubyhoz készült Azure SDK](http://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="e2f8a-171">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

