---
title: "Azure Table storage használata a Python |} Microsoft Docs"
description: "Az Azure Table Storage, amely egy NoSQL-adattár, a strukturált adatok felhőben való tárolásához használható."
services: cosmos-db
documentationcenter: python
author: mimig1
manager: jhubbard
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: cosmos-db
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: mimig
ms.openlocfilehash: 0c46f04786ba4b62bd7ca22c5e25643123e6e136
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-use-table-storage-in-python"></a><span data-ttu-id="a907d-103">A Table storage használata a Python</span><span class="sxs-lookup"><span data-stu-id="a907d-103">How to use Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="a907d-104">Ez az útmutató bemutatja, hogyan hajthat végre az Azure Table storage gyakori helyzetek a Python használatával a [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="a907d-104">This guide shows you how to perform common Azure Table storage scenarios in Python using the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="a907d-105">Az ismertetett forgatókönyvek tartalmaznak, létrehozása és egy tábla törlésével és a entitás lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="a907d-105">The scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="a907d-106">Ebben az oktatóanyagban a forgatókönyvek keresztül működő, miközben Kezdésként tekintse meg a [Python API-referencia az Azure Storage szolgáltatás SDK](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="a907d-106">While working through the scenarios in this tutorial, you may wish to refer to the [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-the-azure-storage-sdk-for-python"></a><span data-ttu-id="a907d-107">Python az Azure Storage SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="a907d-107">Install the Azure Storage SDK for Python</span></span>

<span data-ttu-id="a907d-108">Miután létrehozott egy tárfiókot, a következő lépés a rendszer telepíti a [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="a907d-108">Once you've created a storage account, your next step is to install the [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="a907d-109">Az SDK telepítésével kapcsolatos részletekért tekintse meg a [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) fájlban találhatók a Storage szolgáltatás SDK Python tárház a Githubon.</span><span class="sxs-lookup"><span data-stu-id="a907d-109">For details on installing the SDK, refer to the [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in the Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="a907d-110">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="a907d-110">Create a table</span></span>

<span data-ttu-id="a907d-111">Az Azure Table szolgáltatás a Python használatához importálnia kell a [TableService] [ py_TableService] modul.</span><span class="sxs-lookup"><span data-stu-id="a907d-111">To work with the Azure Table service in Python, you must import the [TableService][py_TableService] module.</span></span> <span data-ttu-id="a907d-112">Mivel lesz a táblaentitásokat dolgozik, akkor is kell a [entitás] [ py_Entity] osztály.</span><span class="sxs-lookup"><span data-stu-id="a907d-112">Since you'll be working with Table entities, you also need the [Entity][py_Entity] class.</span></span> <span data-ttu-id="a907d-113">Adja hozzá ezt a kódot, felső részén a Python-fájl importálása:</span><span class="sxs-lookup"><span data-stu-id="a907d-113">Add this code near the top your Python file to import both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="a907d-114">Hozzon létre egy [TableService] [ py_TableService] objektum, a tárfiók nevét és a fiók kulcsának benyújtása.</span><span class="sxs-lookup"><span data-stu-id="a907d-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="a907d-115">Cserélje le `myaccount` és `mykey` a fióknevet és a kulcs és a hívás [create_table] [ py_create_table] tábla létrehozása az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="a907d-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] to create the table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-to-a-table"></a><span data-ttu-id="a907d-116">Entitás hozzáadása a táblához</span><span class="sxs-lookup"><span data-stu-id="a907d-116">Add an entity to a table</span></span>

<span data-ttu-id="a907d-117">Egy entitás hozzáadásához először létre kell hoznia egy objektumot, amely jelöli az entitásban, majd adja át az objektumot a [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="a907d-117">To add an entity, you first create an object that represents your entity, then pass the object to the [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="a907d-118">A forrásentitás-objektum dictionary vagy típusú objektum lehet [entitás][py_Entity], és határozza meg az entitás nevét és értékeit.</span><span class="sxs-lookup"><span data-stu-id="a907d-118">The entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="a907d-119">Minden entitás tartalmaznia kell a szükséges [PartitionKey és RowKey](#partitionkey-and-rowkey) tulajdonságokat adhat meg az entitás egyéb tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="a907d-119">Every entity must include the required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition to any other properties you define for the entity.</span></span>

<span data-ttu-id="a907d-120">Ebben a példában a szótár-objektumot hoz létre képviselő entitás, majd továbbítja azt a [insert_entity] [ py_insert_entity] módszert veheti fel a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="a907d-120">This example creates a dictionary object representing an entity, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="a907d-121">Ez a példa létrehoz egy [entitás] [ py_Entity] objektumot, majd továbbítja azokat a a [insert_entity] [ py_insert_entity] módszert veheti fel a táblázatban:</span><span class="sxs-lookup"><span data-stu-id="a907d-121">This example creates an [Entity][py_Entity] object, then passes it to the [insert_entity][py_insert_entity] method to add it to the table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash the car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="a907d-122">PartitionKey és RowKey</span><span class="sxs-lookup"><span data-stu-id="a907d-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="a907d-123">Meg kell adni egy **PartitionKey** és egy **RowKey** összes entitás tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a907d-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="a907d-124">Ezek azok az entitások egyedi azonosítói együtt, egy entitás elsődleges kulcsának alkotják.</span><span class="sxs-lookup"><span data-stu-id="a907d-124">These are the unique identifiers of your entities, as together they form the primary key of an entity.</span></span> <span data-ttu-id="a907d-125">Ezek az értékek sokkal gyorsabb, mint bármely más entitás tulajdonságai, mivel csak ezek a Tulajdonságok indexelt kérdezheti használatával kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="a907d-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="a907d-126">A Table szolgáltatás által használt **PartitionKey** intelligens módon táblaentitásokat szét a tárhely csomópontot.</span><span class="sxs-lookup"><span data-stu-id="a907d-126">The Table service uses **PartitionKey** to intelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="a907d-127">Az entitások **PartitionKey** ugyanazon a csomóponton találhatók.</span><span class="sxs-lookup"><span data-stu-id="a907d-127">Entities that have the same  **PartitionKey** are stored on the same node.</span></span> <span data-ttu-id="a907d-128">**RowKey** tartozik, a partíción belül entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a907d-128">**RowKey** is the unique ID of the entity within the partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="a907d-129">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="a907d-129">Update an entity</span></span>

<span data-ttu-id="a907d-130">Frissíti az összes olyan egy entitás tulajdonságértékek, hívja meg a [update_entity] [ py_update_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="a907d-130">To update all of an entity's property values, call the [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="a907d-131">Ez a példa bemutatja, hogyan lecseréli a meglévő entitás frissített verzióra:</span><span class="sxs-lookup"><span data-stu-id="a907d-131">This example shows how to replace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="a907d-132">Ha a frissítendő entitás nem létezik, majd a frissítés művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="a907d-132">If the entity that is being updated doesn't already exist, then the update operation will fail.</span></span> <span data-ttu-id="a907d-133">Ha azt szeretné, hogy egy entitás tárolására hogy megtalálható-e vagy sem, használjon [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="a907d-133">If you want to store an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="a907d-134">A következő példában az első hívás lecseréli a meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="a907d-134">In the following example, the first call will replace the existing entity.</span></span> <span data-ttu-id="a907d-135">A második hívás szúrja be egy új entitást, a megadott PartitionKey nem entitás óta, és RowKey létezik a táblában.</span><span class="sxs-lookup"><span data-stu-id="a907d-135">The second call will insert a new entity, since no entity with the specified PartitionKey and RowKey exists in the table.</span></span>

```python
# Replace the entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out the garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="a907d-136">A [update_entity] [ py_update_entity] módszer a felváltja az összes tulajdonságok és értékek egy meglévő entitás, amely tulajdonságok eltávolítása egy meglévő entitás is használhatja.</span><span class="sxs-lookup"><span data-stu-id="a907d-136">The [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use to remove properties from an existing entity.</span></span> <span data-ttu-id="a907d-137">Használhatja a [merge_entity] [ py_merge_entity] módszer új vagy módosított tulajdonságértékek nem teljesen cseréli le az entitás meglévő entitás frissítheti.</span><span class="sxs-lookup"><span data-stu-id="a907d-137">You can use the [merge_entity][py_merge_entity] method to update an existing entity with new or modified property values without completely replacing the entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="a907d-138">Több entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="a907d-138">Modify multiple entities</span></span>

<span data-ttu-id="a907d-139">A Table szolgáltatás által a kérés atomi feldolgozása érdekében elküldheti a több műveletei együtt egy kötegben.</span><span class="sxs-lookup"><span data-stu-id="a907d-139">To ensure the atomic processing of a request by the Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="a907d-140">Először is a [TableBatch] [ py_TableBatch] osztály több művelet hozzáadása egy tételt.</span><span class="sxs-lookup"><span data-stu-id="a907d-140">First, use the [TableBatch][py_TableBatch] class to add multiple operations to a single batch.</span></span> <span data-ttu-id="a907d-141">Ezután hívja [TableService][py_TableService].[ commit_batch] [ py_commit_batch] egy atomi művelet műveleteknek elküldeni.</span><span class="sxs-lookup"><span data-stu-id="a907d-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] to submit the operations in an atomic operation.</span></span> <span data-ttu-id="a907d-142">Összes entitás kötegelt a módosítani kívánt partícióra kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a907d-142">All entities to be modified in batch must be in the same partition.</span></span>

<span data-ttu-id="a907d-143">Ebben a példában két olyan entitásra kötegben együtt hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="a907d-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean the bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="a907d-144">Kötegek is használható a környezet kezelő szintaxisa:</span><span class="sxs-lookup"><span data-stu-id="a907d-144">Batches can also be used with the context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean the bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="a907d-145">Egy entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="a907d-145">Query for an entity</span></span>

<span data-ttu-id="a907d-146">Egy tábla entitás lekérdezéséhez adja át a PartitionKey és a RowKey a [TableService][py_TableService].[ get_entity] [ py_get_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="a907d-146">To query for an entity in a table, pass its PartitionKey and RowKey to the [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="a907d-147">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="a907d-147">Query a set of entities</span></span>

<span data-ttu-id="a907d-148">Alapján is kereshet az entitások készletének úgy, hogy megadja az egy szűrési karakterláncot a **szűrő** paraméter.</span><span class="sxs-lookup"><span data-stu-id="a907d-148">You can query for a set of entities by supplying a filter string with the **filter** parameter.</span></span> <span data-ttu-id="a907d-149">Ebben a példában megkeresi az összes feladat budapesti PartitionKey a szűrő alkalmazásával:</span><span class="sxs-lookup"><span data-stu-id="a907d-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="a907d-150">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="a907d-150">Query a subset of entity properties</span></span>

<span data-ttu-id="a907d-151">Minden entitás lekérdezésben visszaadja a tulajdonságok is korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="a907d-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="a907d-152">Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű az entitások vagy a eredményhalmazt.</span><span class="sxs-lookup"><span data-stu-id="a907d-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="a907d-153">Használja a **válasszon** paraméter és a kívánt tulajdonságokat nevei küld vissza az ügyfélnek fázis.</span><span class="sxs-lookup"><span data-stu-id="a907d-153">Use the **select** parameter and pass the names of the properties you want returned to the client.</span></span>

<span data-ttu-id="a907d-154">A lekérdezés a következő kódrészlet csak entitások leírását a táblázatban adja vissza.</span><span class="sxs-lookup"><span data-stu-id="a907d-154">The query in the following code returns only the descriptions of entities in the table.</span></span>

> [!NOTE]
> <span data-ttu-id="a907d-155">Az alábbi kódrészletben működik, csak az Azure Storage ellen.</span><span class="sxs-lookup"><span data-stu-id="a907d-155">The following snippet works only against the Azure Storage.</span></span> <span data-ttu-id="a907d-156">A storage emulator által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="a907d-156">It is not supported by the storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="a907d-157">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="a907d-157">Delete an entity</span></span>

<span data-ttu-id="a907d-158">Entitás törlése a PartitionKey és RowKey történő átadásával az [delete_entity] [ py_delete_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="a907d-158">Delete an entity by passing its PartitionKey and RowKey to the [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="a907d-159">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="a907d-159">Delete a table</span></span>

<span data-ttu-id="a907d-160">Ha már nincs szüksége egy táblát, vagy azokat az egységeket belül, a [delete_table] [ py_delete_table] végleg törölni kívánja a táblázatot az Azure Storage metódust.</span><span class="sxs-lookup"><span data-stu-id="a907d-160">If you no longer need a table or any of the entities within it, call the [delete_table][py_delete_table] method to permanently delete the table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="a907d-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a907d-161">Next steps</span></span>

* [<span data-ttu-id="a907d-162">Az Azure Storage SDK for Python API-referencia</span><span class="sxs-lookup"><span data-stu-id="a907d-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="a907d-163">Az Azure Storage Python SDK</span><span class="sxs-lookup"><span data-stu-id="a907d-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="a907d-164">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="a907d-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="a907d-165">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md): egy ingyenes, platformfüggetlen-alkalmazást a Windows, a macOS és a Linux vizuálisan adatok Azure Storage használata.</span><span class="sxs-lookup"><span data-stu-id="a907d-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

[py_commit_batch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.commit_batch
[py_create_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.create_table
[py_delete_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_entity
[py_delete_table]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.delete_table
[py_Entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.models.html#azure.storage.table.models.Entity
[py_get_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.get_entity
[py_insert_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_entity
[py_insert_or_replace_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.insert_or_replace_entity
[py_merge_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.merge_entity
[py_update_entity]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html#azure.storage.table.tableservice.TableService.update_entity
[py_TableService]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tableservice.html
[py_TableBatch]: https://azure-storage.readthedocs.io/en/latest/ref/azure.storage.table.tablebatch.html#azure.storage.table.tablebatch.TableBatch
