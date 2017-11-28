---
title: Azure Table storage Python aaaHow toouse |} Microsoft Docs
description: "Azure Table storage, egy NoSQL-adattár hello felhőbe strukturált adat tárolása."
services: storage
documentationcenter: python
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 7ddb9f3e-4e6d-4103-96e6-f0351d69a17b
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/16/2017
ms.author: marsma
ms.openlocfilehash: fd0e1b05cc12618f348eaf2d85d0dce5ac32702a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-table-storage-in-python"></a><span data-ttu-id="97154-103">Hogyan toouse a Table storage a Python</span><span class="sxs-lookup"><span data-stu-id="97154-103">How toouse Table storage in Python</span></span>

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

<span data-ttu-id="97154-104">Ez az útmutató bemutatja, hogyan tooperform Azure Table storage szabhatják a Python használatával hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="97154-104">This guide shows you how tooperform common Azure Table storage scenarios in Python using hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="97154-105">hello jelzett esetek létrehozása és egy tábla törlésével és a entitás lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="97154-105">hello scenarios covered include creating and deleting a table, and inserting and querying entities.</span></span>

<span data-ttu-id="97154-106">Ebben az oktatóanyagban hello forgatókönyvek keresztül működő, miközben Kezdésként toorefer toohello [Python API-referencia az Azure Storage szolgáltatás SDK](https://azure-storage.readthedocs.io/en/latest/index.html).</span><span class="sxs-lookup"><span data-stu-id="97154-106">While working through hello scenarios in this tutorial, you may wish toorefer toohello [Azure Storage SDK for Python API reference](https://azure-storage.readthedocs.io/en/latest/index.html).</span></span>

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a><span data-ttu-id="97154-107">Python hello Azure Storage szolgáltatás SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="97154-107">Install hello Azure Storage SDK for Python</span></span>

<span data-ttu-id="97154-108">Miután létrehozott egy tárfiókot, a következő lépésre-e tooinstall hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span><span class="sxs-lookup"><span data-stu-id="97154-108">Once you've created a storage account, your next step is tooinstall hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python).</span></span> <span data-ttu-id="97154-109">A telepítésének részleteiről hello SDK, tekintse meg a toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) Python tárház a Githubon hello Storage szolgáltatás SDK fájlban.</span><span class="sxs-lookup"><span data-stu-id="97154-109">For details on installing hello SDK, refer toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) file in hello Storage SDK for Python repository on GitHub.</span></span>

## <a name="create-a-table"></a><span data-ttu-id="97154-110">Tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="97154-110">Create a table</span></span>

<span data-ttu-id="97154-111">az Azure Table szolgáltatás a Python hello toowork, importálnia kell a hello [TableService] [ py_TableService] modul.</span><span class="sxs-lookup"><span data-stu-id="97154-111">toowork with hello Azure Table service in Python, you must import hello [TableService][py_TableService] module.</span></span> <span data-ttu-id="97154-112">Mivel lesz a táblaentitásokat dolgozik, akkor is kell hello [entitás] [ py_Entity] osztály.</span><span class="sxs-lookup"><span data-stu-id="97154-112">Since you'll be working with Table entities, you also need hello [Entity][py_Entity] class.</span></span> <span data-ttu-id="97154-113">Ez a kód hello felső közelében a Python fájl tooimport mindkét hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="97154-113">Add this code near hello top your Python file tooimport both:</span></span>

```python
from azure.storage.table import TableService, Entity
```

<span data-ttu-id="97154-114">Hozzon létre egy [TableService] [ py_TableService] objektum, a tárfiók nevét és a fiók kulcsának benyújtása.</span><span class="sxs-lookup"><span data-stu-id="97154-114">Create a [TableService][py_TableService] object, passing in your storage account name and account key.</span></span> <span data-ttu-id="97154-115">Cserélje le `myaccount` és `mykey` a fióknevet és a kulcs és a hívás [create_table] [ py_create_table] toocreate hello tábla az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="97154-115">Replace `myaccount` and `mykey` with your account name and key, and call [create_table][py_create_table] toocreate hello table in Azure Storage.</span></span>

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a><span data-ttu-id="97154-116">Egy entitás tooa táblázat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="97154-116">Add an entity tooa table</span></span>

<span data-ttu-id="97154-117">egy entitás tooadd, először létre kell hoznia az entitásban, majd fázis hello objektum toohello képviselő objektum [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="97154-117">tooadd an entity, you first create an object that represents your entity, then pass hello object toohello [TableService][py_TableService].[insert_entity][py_insert_entity] method.</span></span> <span data-ttu-id="97154-118">hello entitásobjektumot dictionary vagy típusú objektum lehet [entitás][py_Entity], és határozza meg az entitás nevét és értékeit.</span><span class="sxs-lookup"><span data-stu-id="97154-118">hello entity object can be a dictionary or an object of type [Entity][py_Entity], and defines your entity's property names and values.</span></span> <span data-ttu-id="97154-119">Minden entitás tartalmaznia kell a szükséges hello [PartitionKey és RowKey](#partitionkey-and-rowkey) tulajdonságok, továbbá tooany más tulajdonságokat megadhatja hello entitás.</span><span class="sxs-lookup"><span data-stu-id="97154-119">Every entity must include hello required [PartitionKey and RowKey](#partitionkey-and-rowkey) properties, in addition tooany other properties you define for hello entity.</span></span>

<span data-ttu-id="97154-120">Ez a példa létrehoz egy szótár objektumot képviselő entitás, majd továbbítja azokat toohello [insert_entity] [ py_insert_entity] metódus tooadd azt toohello tábla:</span><span class="sxs-lookup"><span data-stu-id="97154-120">This example creates a dictionary object representing an entity, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

<span data-ttu-id="97154-121">Ez a példa létrehoz egy [entitás] [ py_Entity] objektumot, majd továbbítja azokat toohello [insert_entity] [ py_insert_entity] metódus tooadd azt toohello tábla:</span><span class="sxs-lookup"><span data-stu-id="97154-121">This example creates an [Entity][py_Entity] object, then passes it toohello [insert_entity][py_insert_entity] method tooadd it toohello table:</span></span>

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a><span data-ttu-id="97154-122">PartitionKey és RowKey</span><span class="sxs-lookup"><span data-stu-id="97154-122">PartitionKey and RowKey</span></span>

<span data-ttu-id="97154-123">Meg kell adni egy **PartitionKey** és egy **RowKey** összes entitás tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="97154-123">You must specify both a **PartitionKey** and a **RowKey** property for every entity.</span></span> <span data-ttu-id="97154-124">Ezek a hello egyedi azonosítói az entitások szerint együtt alkotják hello egy entitás elsődleges kulcsát.</span><span class="sxs-lookup"><span data-stu-id="97154-124">These are hello unique identifiers of your entities, as together they form hello primary key of an entity.</span></span> <span data-ttu-id="97154-125">Ezek az értékek sokkal gyorsabb, mint bármely más entitás tulajdonságai, mivel csak ezek a Tulajdonságok indexelt kérdezheti használatával kérdezheti le.</span><span class="sxs-lookup"><span data-stu-id="97154-125">You can query using these values much faster than you can query any other entity properties because only these properties are indexed.</span></span>

<span data-ttu-id="97154-126">Table szolgáltatás által használt hello **PartitionKey** toointelligently táblaentitásokat szét a tárhely csomópontot.</span><span class="sxs-lookup"><span data-stu-id="97154-126">hello Table service uses **PartitionKey** toointelligently distribute table entities across storage nodes.</span></span> <span data-ttu-id="97154-127">Rendelkező entitásokat hello azonos **PartitionKey** hello tárolt ugyanahhoz a csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="97154-127">Entities that have hello same  **PartitionKey** are stored on hello same node.</span></span> <span data-ttu-id="97154-128">**RowKey** hello tartozik hello partíción belül hello entitás egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="97154-128">**RowKey** is hello unique ID of hello entity within hello partition it belongs to.</span></span>

## <a name="update-an-entity"></a><span data-ttu-id="97154-129">Egy entitás frissítése</span><span class="sxs-lookup"><span data-stu-id="97154-129">Update an entity</span></span>

<span data-ttu-id="97154-130">minden tooupdate egy entitás tulajdonságértékek, hívja az hello [update_entity] [ py_update_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="97154-130">tooupdate all of an entity's property values, call hello [update_entity][py_update_entity] method.</span></span> <span data-ttu-id="97154-131">A példa bemutatja, hogyan tooreplace meglévő entitás frissített verzióra:</span><span class="sxs-lookup"><span data-stu-id="97154-131">This example shows how tooreplace an existing entity with an updated version:</span></span>

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

<span data-ttu-id="97154-132">Ha a frissítés alatt álló hello entitás nem létezik, majd hello frissítési művelet sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="97154-132">If hello entity that is being updated doesn't already exist, then hello update operation will fail.</span></span> <span data-ttu-id="97154-133">Egy entitás toostore azt szeretné, hogy létezik-e, ha [insert_or_replace_entity][py_insert_or_replace_entity].</span><span class="sxs-lookup"><span data-stu-id="97154-133">If you want toostore an entity whether it exists or not, use [insert_or_replace_entity][py_insert_or_replace_entity].</span></span> <span data-ttu-id="97154-134">A következő példa hello első hívás hello lecseréli hello meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="97154-134">In hello following example, hello first call will replace hello existing entity.</span></span> <span data-ttu-id="97154-135">hello második hívás szúrja be egy új entitást, mivel nem entitás hello az adott PartitionKey és RowKey létezik hello táblában.</span><span class="sxs-lookup"><span data-stu-id="97154-135">hello second call will insert a new entity, since no entity with hello specified PartitionKey and RowKey exists in hello table.</span></span>

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> <span data-ttu-id="97154-136">Hello [update_entity] [ py_update_entity] módszer a felváltja az összes tulajdonságok és értékek egy meglévő entitás, amely is használja a meglévő entitás tooremove tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="97154-136">hello [update_entity][py_update_entity] method replaces all properties and values of an existing entity, which you can also use tooremove properties from an existing entity.</span></span> <span data-ttu-id="97154-137">Használhatja a hello [merge_entity] [ py_merge_entity] metódus tooupdate rendelkező új vagy módosított tulajdonságértékek nem teljesen cseréli le a hello entitás meglévő entitás.</span><span class="sxs-lookup"><span data-stu-id="97154-137">You can use hello [merge_entity][py_merge_entity] method tooupdate an existing entity with new or modified property values without completely replacing hello entity.</span></span>

## <a name="modify-multiple-entities"></a><span data-ttu-id="97154-138">Több entitás módosítása</span><span class="sxs-lookup"><span data-stu-id="97154-138">Modify multiple entities</span></span>

<span data-ttu-id="97154-139">tooensure kérelem atomi feldolgozási hello hello Table szolgáltatás által, elküldheti a több műveletei együtt egy kötegben.</span><span class="sxs-lookup"><span data-stu-id="97154-139">tooensure hello atomic processing of a request by hello Table service, you can submit multiple operations together in a batch.</span></span> <span data-ttu-id="97154-140">Először használja a hello [TableBatch] [ py_TableBatch] tooadd osztály több műveletek tooa kötegben.</span><span class="sxs-lookup"><span data-stu-id="97154-140">First, use hello [TableBatch][py_TableBatch] class tooadd multiple operations tooa single batch.</span></span> <span data-ttu-id="97154-141">Ezután hívja [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello műveletek atomi művelet.</span><span class="sxs-lookup"><span data-stu-id="97154-141">Next, call [TableService][py_TableService].[commit_batch][py_commit_batch] toosubmit hello operations in an atomic operation.</span></span> <span data-ttu-id="97154-142">Kötegelt ugyanúgy módosított összes entitások toobe kell hello egyazon partícióra kerüljenek.</span><span class="sxs-lookup"><span data-stu-id="97154-142">All entities toobe modified in batch must be in hello same partition.</span></span>

<span data-ttu-id="97154-143">Ebben a példában két olyan entitásra kötegben együtt hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="97154-143">This example adds two entities together in a batch:</span></span>

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

<span data-ttu-id="97154-144">Kötegek hello környezetben szintaxisára együtt is használható:</span><span class="sxs-lookup"><span data-stu-id="97154-144">Batches can also be used with hello context manager syntax:</span></span>

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a><span data-ttu-id="97154-145">Egy entitás lekérdezése</span><span class="sxs-lookup"><span data-stu-id="97154-145">Query for an entity</span></span>

<span data-ttu-id="97154-146">egy entitás tooquery adja át a PartitionKey és RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="97154-146">tooquery for an entity in a table, pass its PartitionKey and RowKey toohello [TableService][py_TableService].[get_entity][py_get_entity] method.</span></span>

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a><span data-ttu-id="97154-147">Az entitások készletének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="97154-147">Query a set of entities</span></span>

<span data-ttu-id="97154-148">Az entitások készletének úgy, hogy megadja a szűrési karakterláncot a hello kérdezhetők **szűrő** paraméter.</span><span class="sxs-lookup"><span data-stu-id="97154-148">You can query for a set of entities by supplying a filter string with hello **filter** parameter.</span></span> <span data-ttu-id="97154-149">Ebben a példában megkeresi az összes feladat budapesti PartitionKey a szűrő alkalmazásával:</span><span class="sxs-lookup"><span data-stu-id="97154-149">This example finds all tasks in Seattle by applying a filter on PartitionKey:</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a><span data-ttu-id="97154-150">Az entitástulajdonságok egy részének lekérdezése</span><span class="sxs-lookup"><span data-stu-id="97154-150">Query a subset of entity properties</span></span>

<span data-ttu-id="97154-151">Minden entitás lekérdezésben visszaadja a tulajdonságok is korlátozhatja.</span><span class="sxs-lookup"><span data-stu-id="97154-151">You can also restrict which properties are returned for each entity in a query.</span></span> <span data-ttu-id="97154-152">Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű az entitások vagy a eredményhalmazt.</span><span class="sxs-lookup"><span data-stu-id="97154-152">This technique, called *projection*, reduces bandwidth and can improve query performance, especially for large entities or result sets.</span></span> <span data-ttu-id="97154-153">Használjon hello **válasszon** kívánt hello tulajdonságainak paraméter és pass hello nevében visszaadott toohello ügyfél.</span><span class="sxs-lookup"><span data-stu-id="97154-153">Use hello **select** parameter and pass hello names of hello properties you want returned toohello client.</span></span>

<span data-ttu-id="97154-154">a következő kód hello hello lekérdezést entitások csak hello leírását hello tábla adja vissza.</span><span class="sxs-lookup"><span data-stu-id="97154-154">hello query in hello following code returns only hello descriptions of entities in hello table.</span></span>

> [!NOTE]
> <span data-ttu-id="97154-155">a következő kódrészletet works csak elleni hello Azure Storage hello.</span><span class="sxs-lookup"><span data-stu-id="97154-155">hello following snippet works only against hello Azure Storage.</span></span> <span data-ttu-id="97154-156">Hello storage emulator által nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="97154-156">It is not supported by hello storage emulator.</span></span>

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a><span data-ttu-id="97154-157">Entitás törlése</span><span class="sxs-lookup"><span data-stu-id="97154-157">Delete an entity</span></span>

<span data-ttu-id="97154-158">Entitás törlése úgy, hogy a PartitionKey és RowKey toohello [delete_entity] [ py_delete_entity] metódust.</span><span class="sxs-lookup"><span data-stu-id="97154-158">Delete an entity by passing its PartitionKey and RowKey toohello [delete_entity][py_delete_entity] method.</span></span>

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a><span data-ttu-id="97154-159">Tábla törlése</span><span class="sxs-lookup"><span data-stu-id="97154-159">Delete a table</span></span>

<span data-ttu-id="97154-160">Ha már nincs szüksége egy táblázatot vagy bármelyik hello entitások belül, hívja hello [delete_table] [ py_delete_table] metódus toopermanently hello tábla törlése az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="97154-160">If you no longer need a table or any of hello entities within it, call hello [delete_table][py_delete_table] method toopermanently delete hello table from Azure Storage.</span></span>

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a><span data-ttu-id="97154-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97154-161">Next steps</span></span>

* [<span data-ttu-id="97154-162">Az Azure Storage SDK for Python API-referencia</span><span class="sxs-lookup"><span data-stu-id="97154-162">Azure Storage SDK for Python API reference</span></span>](https://azure-storage.readthedocs.io/en/latest/index.html)
* [<span data-ttu-id="97154-163">Az Azure Storage Python SDK</span><span class="sxs-lookup"><span data-stu-id="97154-163">Azure Storage SDK for Python</span></span>](https://github.com/Azure/azure-storage-python)
* [<span data-ttu-id="97154-164">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="97154-164">Python Developer Center</span></span>](https://azure.microsoft.com/develop/python/)
* <span data-ttu-id="97154-165">[A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md): egy ingyenes, platformfüggetlen-alkalmazást a Windows, a macOS és a Linux vizuálisan adatok Azure Storage használata.</span><span class="sxs-lookup"><span data-stu-id="97154-165">[Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md): A free, cross-platform application for working visually with Azure Storage data on Windows, macOS, and Linux.</span></span>

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
