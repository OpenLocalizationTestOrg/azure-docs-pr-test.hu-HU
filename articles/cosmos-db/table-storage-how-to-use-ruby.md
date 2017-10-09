---
title: "Azure Table Storage-ának Ruby aaaHow toouse |} Microsoft Docs"
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
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
ms.openlocfilehash: 2f9eb5a9160b551d6d1d198869787070c402b1d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-table-storage-from-ruby"></a><span data-ttu-id="0e02a-103">Hogyan toouse Azure Table Storage-ának Ruby</span><span class="sxs-lookup"><span data-stu-id="0e02a-103">How toouse Azure Table Storage from Ruby</span></span>
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

## <a name="overview"></a><span data-ttu-id="0e02a-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0e02a-104">Overview</span></span>
<span data-ttu-id="0e02a-105">Ez az útmutató bemutatja, hogyan tooperform szolgáltatást használó általános forgatókönyvhöz hello Azure Table szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0e02a-105">This guide shows you how tooperform common scenarios using hello Azure Table service.</span></span> <span data-ttu-id="0e02a-106">hello minták hello Ruby API segítségével készül.</span><span class="sxs-lookup"><span data-stu-id="0e02a-106">hello samples are written using hello Ruby API.</span></span> <span data-ttu-id="0e02a-107">hello tárgyalt forgatókönyvekben szerepel a **létrehozása egy tábla törlésével, és a tábla az entitások lekérdezése**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-107">hello scenarios covered include **creating and deleting a table, inserting and querying entities in a table**.</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="create-a-ruby-application"></a><span data-ttu-id="0e02a-108">Ruby-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e02a-108">Create a Ruby application</span></span>
<span data-ttu-id="0e02a-109">Hogyan toocreate Ruby alkalmazás, lásd: [Ruby sínek webalkalmazás egy Azure virtuális gépen](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span><span class="sxs-lookup"><span data-stu-id="0e02a-109">For instructions how toocreate a Ruby application, see [Ruby on Rails Web application on an Azure VM](../virtual-machines/linux/classic/virtual-machines-linux-classic-ruby-rails-web-app.md).</span></span>

## <a name="configure-your-application-tooaccess-storage"></a><span data-ttu-id="0e02a-110">Az alkalmazás tooaccess tároló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0e02a-110">Configure your application tooaccess Storage</span></span>
<span data-ttu-id="0e02a-111">Azure Storage toouse, és van szükség toodownload használata hello Ruby azure csomagot tartalmaz, a felhasználók kényelme érdekében szalagtár szerepel, amely hello Storage REST szolgáltatásokkal kommunikálni.</span><span class="sxs-lookup"><span data-stu-id="0e02a-111">toouse Azure Storage, you need toodownload and use hello Ruby azure package which includes a set of convenience libraries that communicate with hello Storage REST services.</span></span>

### <a name="use-rubygems-tooobtain-hello-package"></a><span data-ttu-id="0e02a-112">RubyGems tooobtain hello csomag használata</span><span class="sxs-lookup"><span data-stu-id="0e02a-112">Use RubyGems tooobtain hello package</span></span>
1. <span data-ttu-id="0e02a-113">Használjon például egy parancssori felületet **PowerShell** (Windows), **Terminálszolgáltatások** (Mac), vagy **Bash** (Unix).</span><span class="sxs-lookup"><span data-stu-id="0e02a-113">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Unix).</span></span>
2. <span data-ttu-id="0e02a-114">Típus **gem telepítése azure** hello parancs ablak tooinstall hello gem és függőségeit.</span><span class="sxs-lookup"><span data-stu-id="0e02a-114">Type **gem install azure** in hello command window tooinstall hello gem and dependencies.</span></span>

### <a name="import-hello-package"></a><span data-ttu-id="0e02a-115">Hello csomag importálása</span><span class="sxs-lookup"><span data-stu-id="0e02a-115">Import hello package</span></span>
<span data-ttu-id="0e02a-116">Kedvenc szövegszerkesztőjével használatához adja hozzá a következő hello toouse tárolási célhelyeként Ruby fájl elejéhez toohello hello:</span><span class="sxs-lookup"><span data-stu-id="0e02a-116">Use your favorite text editor, add hello following toohello top of hello Ruby file where you intend toouse Storage:</span></span>

```ruby
require "azure"
```

## <a name="set-up-an-azure-storage-connection"></a><span data-ttu-id="0e02a-117">Egy Azure Storage-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="0e02a-117">Set up an Azure Storage connection</span></span>
<span data-ttu-id="0e02a-118">hello azure modul olvassák hello környezeti változók **AZURE\_tárolási\_fiók** és **AZURE\_tárolási\_hozzáférés\_kulcs**információ tooconnect tooyour Azure Storage-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="0e02a-118">hello azure module will read hello environment variables **AZURE\_STORAGE\_ACCOUNT** and **AZURE\_STORAGE\_ACCESS\_KEY** for information required tooconnect tooyour Azure Storage account.</span></span> <span data-ttu-id="0e02a-119">Ha ezek a környezeti változók nem, meg kell adnia hello fiók használata előtt **Azure::TableService** a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="0e02a-119">If these environment variables are not set, you must specify hello account information before using **Azure::TableService** with hello following code:</span></span>

```ruby
Azure.config.storage_account_name = "<your azure storage account>"
Azure.config.storage_access_key = "<your azure storage access key>"
```

<span data-ttu-id="0e02a-120">tooobtain ezeket az értékeket a klasszikus vagy erőforrás-kezelő tárolási fiók hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="0e02a-120">tooobtain these values from a classic or Resource Manager storage account in hello Azure portal:</span></span>

1. <span data-ttu-id="0e02a-121">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="0e02a-121">Log in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="0e02a-122">Keresse meg a kívánt toouse toohello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="0e02a-122">Navigate toohello storage account you want toouse.</span></span>
3. <span data-ttu-id="0e02a-123">Kattintson a jobb oldali hello hello beállítási paneljén **hívóbetűk**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-123">In hello Settings blade on hello right, click **Access Keys**.</span></span>
4. <span data-ttu-id="0e02a-124">Hello elérési kulcsok paneljén megjelenő láthatja, hello hozzáférési kulcs 1 és 2 elérési kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0e02a-124">In hello Access keys blade that appears, you'll see hello access key 1 and access key 2.</span></span> <span data-ttu-id="0e02a-125">Ezek bármelyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="0e02a-125">You can use either of these.</span></span>
5. <span data-ttu-id="0e02a-126">Kattintson a hello másolás ikon toocopy hello kulcs toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="0e02a-126">Click hello copy icon toocopy hello key toohello clipboard.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="0e02a-127">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="0e02a-127">Create a table</span></span>
<span data-ttu-id="0e02a-128">Hello **Azure::TableService** objektum lehetővé teszi, hogy a táblákat és entitásokat.</span><span class="sxs-lookup"><span data-stu-id="0e02a-128">hello **Azure::TableService** object lets you work with tables and entities.</span></span> <span data-ttu-id="0e02a-129">egy tábla toocreate hello használata **létrehozása\_table()** metódust.</span><span class="sxs-lookup"><span data-stu-id="0e02a-129">toocreate a table, use hello **create\_table()** method.</span></span> <span data-ttu-id="0e02a-130">hello alábbi példa létrehoz egy táblát, vagy nyomtatása hello hiba, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="0e02a-130">hello following example creates a table or prints hello error if there is any.</span></span>

```ruby
azure_table_service = Azure::TableService.new
begin
    azure_table_service.create_table("testtable")
rescue
    puts $!
end
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="0e02a-131">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0e02a-131">Add an entity tooa table</span></span>
<span data-ttu-id="0e02a-132">egy entitás tooadd először létre kell hoznia egy ujjlenyomat-objektumot, amely meghatározza az entitás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="0e02a-132">tooadd an entity, first create a hash object that defines your entity properties.</span></span> <span data-ttu-id="0e02a-133">Vegye figyelembe, hogy minden entitás meg kell adnia egy **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-133">Note that for every entity you must specify a **PartitionKey** and **RowKey**.</span></span> <span data-ttu-id="0e02a-134">Ezek hello egyedi azonosítói az entitások, és sokkal gyorsabb, mint a többi tulajdonság lekérdezett érték.</span><span class="sxs-lookup"><span data-stu-id="0e02a-134">These are hello unique identifiers of your entities, and are values that can be queried much faster than your other properties.</span></span> <span data-ttu-id="0e02a-135">Használja az Azure Storage **PartitionKey** tooautomatically hello tábla entitások terjesztése számos tárolási csomópont feladatait.</span><span class="sxs-lookup"><span data-stu-id="0e02a-135">Azure Storage uses **PartitionKey** tooautomatically distribute hello table's entities over many storage nodes.</span></span> <span data-ttu-id="0e02a-136">Az entitások hello azonos **PartitionKey** hello tárolt ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="0e02a-136">Entities with hello same **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="0e02a-137">Hello **RowKey** hello tartozik hello partíción belül hello entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0e02a-137">hello **RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

```ruby
entity = { "content" => "test entity",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.insert_entity("testtable", entity)
```

## <a name="update-an-entity"></a><span data-ttu-id="0e02a-138">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="0e02a-138">Update an entity</span></span>
<span data-ttu-id="0e02a-139">Van több módszer érhető el tooupdate meglévő entitás:</span><span class="sxs-lookup"><span data-stu-id="0e02a-139">There are multiple methods available tooupdate an existing entity:</span></span>

* <span data-ttu-id="0e02a-140">**frissítés\_entity():** cserélje ki egy meglévő entitás frissítése.</span><span class="sxs-lookup"><span data-stu-id="0e02a-140">**update\_entity():** Update an existing entity by replacing it.</span></span>
* <span data-ttu-id="0e02a-141">**Egyesítési\_entity():** meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="0e02a-141">**merge\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span>
* <span data-ttu-id="0e02a-142">**Helyezze be\_vagy\_egyesítési\_entity():** cserélje ki egy meglévő entitás frissíti.</span><span class="sxs-lookup"><span data-stu-id="0e02a-142">**insert\_or\_merge\_entity():** Updates an existing entity by replacing it.</span></span> <span data-ttu-id="0e02a-143">Ha nem entitás létezik, egy új szúrja be:</span><span class="sxs-lookup"><span data-stu-id="0e02a-143">If no entity exists, a new one will be inserted:</span></span>
* <span data-ttu-id="0e02a-144">**Helyezze be\_vagy\_cserélje le\_entity():** meglévő entitás frissíti új tulajdonságértékek egyesíti hello meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="0e02a-144">**insert\_or\_replace\_entity():** Updates an existing entity by merging new property values into hello existing entity.</span></span> <span data-ttu-id="0e02a-145">Ha nem entitás létezik, a program beszúrja egy újat.</span><span class="sxs-lookup"><span data-stu-id="0e02a-145">If no entity exists, a new one will be inserted.</span></span>

<span data-ttu-id="0e02a-146">hello következő példa bemutatja egy entitás használatával frissítése **frissítése\_entity()**:</span><span class="sxs-lookup"><span data-stu-id="0e02a-146">hello following example demonstrates updating an entity using **update\_entity()**:</span></span>

```ruby
entity = { "content" => "test entity with updated content",
    :PartitionKey => "test-partition-key", :RowKey => "1" }
azure_table_service.update_entity("testtable", entity)
```

<span data-ttu-id="0e02a-147">A **frissítése\_entity()** és **egyesítési\_entity()**, ha frissített hello entitás nem létezik, hello frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="0e02a-147">With **update\_entity()** and **merge\_entity()**, if hello entity that you are updating doesn't exist then hello update operation will fail.</span></span> <span data-ttu-id="0e02a-148">Ezért ha toostore egy entitás, függetlenül attól, hogy létezik-e, Ehelyett használjon **beszúrása\_vagy\_cserélje le\_entity()** vagy **beszúrása\_vagy \_egyesítési\_entity()**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-148">Therefore if you wish toostore an entity regardless of whether it already exists, you should instead use **insert\_or\_replace\_entity()** or **insert\_or\_merge\_entity()**.</span></span>

## <a name="work-with-groups-of-entities"></a><span data-ttu-id="0e02a-149">Az entitások csoportok használata</span><span class="sxs-lookup"><span data-stu-id="0e02a-149">Work with groups of entities</span></span>
<span data-ttu-id="0e02a-150">Néha azt teszi logika toosubmit több művelet együtt egy kötegelt tooensure atomi hello kiszolgáló általi feldolgozás alatt.</span><span class="sxs-lookup"><span data-stu-id="0e02a-150">Sometimes it makes sense toosubmit multiple operations together in a batch tooensure atomic processing by hello server.</span></span> <span data-ttu-id="0e02a-151">tooaccomplish, hogy először létre kell hoznia egy **kötegelt** objektumot, majd hello **hajtható végre\_batch()** metódusa **TableService**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-151">tooaccomplish that, you first create a **Batch** object and then use hello **execute\_batch()** method on **TableService**.</span></span> <span data-ttu-id="0e02a-152">hello következő példa bemutatja, két olyan entitásra RowKey 2 és 3 kötegben elküldése.</span><span class="sxs-lookup"><span data-stu-id="0e02a-152">hello following example demonstrates submitting two entities with RowKey 2 and 3 in a batch.</span></span> <span data-ttu-id="0e02a-153">Figyelje meg, hogy az informatikai, csak rendelkező entitások works hello azonos PartitionKey.</span><span class="sxs-lookup"><span data-stu-id="0e02a-153">Notice that it only works for entities with hello same PartitionKey.</span></span>

```ruby
azure_table_service = Azure::TableService.new
batch = Azure::Storage::Table::Batch.new("testtable",
    "test-partition-key") do
    insert "2", { "content" => "new content 2" }
    insert "3", { "content" => "new content 3" }
end
results = azure_table_service.execute_batch(batch)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="0e02a-154">Egy entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="0e02a-154">Query for an entity</span></span>
<span data-ttu-id="0e02a-155">egy tábla, használjon hello entitás tooquery **beolvasása\_entity()** metódus úgy, hogy hello táblanév, **PartitionKey** és **RowKey**.</span><span class="sxs-lookup"><span data-stu-id="0e02a-155">tooquery an entity in a table, use hello **get\_entity()** method, by passing hello table name, **PartitionKey** and **RowKey**.</span></span>

```ruby
result = azure_table_service.get_entity("testtable", "test-partition-key",
    "1")
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="0e02a-156">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="0e02a-156">Query a set of entities</span></span>
<span data-ttu-id="0e02a-157">az egy tábla entitások készletének tooquery létrehozása lekérdezés ujjlenyomat-objektumot, és hello használata **lekérdezés\_entities()** metódust.</span><span class="sxs-lookup"><span data-stu-id="0e02a-157">tooquery a set of entities in a table, create a query hash object and use hello **query\_entities()** method.</span></span> <span data-ttu-id="0e02a-158">hello következő példa bemutatja a hello entitásokhoz hello első azonos **PartitionKey**:</span><span class="sxs-lookup"><span data-stu-id="0e02a-158">hello following example demonstrates getting all hello entities with hello same **PartitionKey**:</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'" }
result, token = azure_table_service.query_entities("testtable", query)
```

> [!NOTE]
> <span data-ttu-id="0e02a-159">Ha hello eredménykészlet túl nagy a lekérdezés egyetlen tooreturn, a folytatási kód visszaadott amellyel tooretrieve következő lapjain.</span><span class="sxs-lookup"><span data-stu-id="0e02a-159">If hello result set is too large for a single query tooreturn, a continuation token will be returned which you can use tooretrieve subsequent pages.</span></span>
>
>

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="0e02a-160">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="0e02a-160">Query a subset of entity properties</span></span>
<span data-ttu-id="0e02a-161">A lekérdezéstábla tooa pár tulajdonságok kérhetnek le egy entitás.</span><span class="sxs-lookup"><span data-stu-id="0e02a-161">A query tooa table can retrieve just a few properties from an entity.</span></span> <span data-ttu-id="0e02a-162">Ez a módszer, "leképezés" nevű csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű entitásokat.</span><span class="sxs-lookup"><span data-stu-id="0e02a-162">This technique, called "projection", reduces bandwidth and can improve query performance, especially for large entities.</span></span> <span data-ttu-id="0e02a-163">Használjon hello select záradékban és pass hello nevei hello tulajdonságai milyen toobring toohello ügyfél keresztül.</span><span class="sxs-lookup"><span data-stu-id="0e02a-163">Use hello select clause and pass hello names of hello properties you would like toobring over toohello client.</span></span>

```ruby
query = { :filter => "PartitionKey eq 'test-partition-key'",
    :select => ["content"] }
result, token = azure_table_service.query_entities("testtable", query)
```

## <a name="delete-an-entity"></a><span data-ttu-id="0e02a-164">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="0e02a-164">Delete an entity</span></span>
<span data-ttu-id="0e02a-165">egy entitás toodelete hello használata **törlése\_entity()** metódust.</span><span class="sxs-lookup"><span data-stu-id="0e02a-165">toodelete an entity, use hello **delete\_entity()** method.</span></span> <span data-ttu-id="0e02a-166">Hello nevében hello entitás, hello PartitionKey és hello entitás RowKey tartalmazó hello tábla toopass van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0e02a-166">You need toopass in hello name of hello table which contains hello entity, hello PartitionKey and RowKey of hello entity.</span></span>

```ruby
azure_table_service.delete_entity("testtable", "test-partition-key", "1")
```

## <a name="delete-a-table"></a><span data-ttu-id="0e02a-167">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="0e02a-167">Delete a table</span></span>
<span data-ttu-id="0e02a-168">egy tábla toodelete hello használata **törlése\_table()** metódus és pass hello nevű hello toodelete kívánt táblát.</span><span class="sxs-lookup"><span data-stu-id="0e02a-168">toodelete a table, use hello **delete\_table()** method and pass in hello name of hello table you want toodelete.</span></span>

```ruby
azure_table_service.delete_table("testtable")
```

## <a name="next-steps"></a><span data-ttu-id="0e02a-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0e02a-169">Next steps</span></span>

* <span data-ttu-id="0e02a-170">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md) egy ingyenes, különálló alkalmazás, amely lehetővé teszi toowork vizuálisan macOS, Linux és a Windows Azure Storage-adatokat a Microsoft.</span><span class="sxs-lookup"><span data-stu-id="0e02a-170">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) is a free, standalone app from Microsoft that enables you toowork visually with Azure Storage data on Windows, macOS, and Linux.</span></span>
* <span data-ttu-id="0e02a-171">[Rubyhoz készült Azure SDK](http://github.com/WindowsAzure/azure-sdk-for-ruby) GitHub tárházából</span><span class="sxs-lookup"><span data-stu-id="0e02a-171">[Azure SDK for Ruby](http://github.com/WindowsAzure/azure-sdk-for-ruby) repository on GitHub</span></span>

