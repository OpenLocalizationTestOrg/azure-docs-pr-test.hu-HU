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
# <a name="how-toouse-table-storage-in-python"></a>Hogyan toouse a Table storage a Python

[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-langsoon-tip-include](../../includes/storage-table-cosmos-db-langsoon-tip-include.md)]

Ez az útmutató bemutatja, hogyan tooperform Azure Table storage szabhatják a Python használatával hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python). hello jelzett esetek létrehozása és egy tábla törlésével és a entitás lekérdezése.

Ebben az oktatóanyagban hello forgatókönyvek keresztül működő, miközben Kezdésként toorefer toohello [Python API-referencia az Azure Storage szolgáltatás SDK](https://azure-storage.readthedocs.io/en/latest/index.html).

[!INCLUDE [storage-table-concepts-include](../../includes/storage-table-concepts-include.md)]

[!INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

## <a name="install-hello-azure-storage-sdk-for-python"></a>Python hello Azure Storage szolgáltatás SDK telepítése

Miután létrehozott egy tárfiókot, a következő lépésre-e tooinstall hello [Microsoft Azure Storage SDK for Python](https://github.com/Azure/azure-storage-python). A telepítésének részleteiről hello SDK, tekintse meg a toohello [README.rst](https://github.com/Azure/azure-storage-python/blob/master/README.rst) Python tárház a Githubon hello Storage szolgáltatás SDK fájlban.

## <a name="create-a-table"></a>Tábla létrehozása

az Azure Table szolgáltatás a Python hello toowork, importálnia kell a hello [TableService] [ py_TableService] modul. Mivel lesz a táblaentitásokat dolgozik, akkor is kell hello [entitás] [ py_Entity] osztály. Ez a kód hello felső közelében a Python fájl tooimport mindkét hozzáadása:

```python
from azure.storage.table import TableService, Entity
```

Hozzon létre egy [TableService] [ py_TableService] objektum, a tárfiók nevét és a fiók kulcsának benyújtása. Cserélje le `myaccount` és `mykey` a fióknevet és a kulcs és a hívás [create_table] [ py_create_table] toocreate hello tábla az Azure Storage.

```python
table_service = TableService(account_name='myaccount', account_key='mykey')

table_service.create_table('tasktable')
```

## <a name="add-an-entity-tooa-table"></a>Egy entitás tooa táblázat hozzáadása

egy entitás tooadd, először létre kell hoznia az entitásban, majd fázis hello objektum toohello képviselő objektum [TableService][py_TableService].[ insert_entity] [ py_insert_entity] metódust. hello entitásobjektumot dictionary vagy típusú objektum lehet [entitás][py_Entity], és határozza meg az entitás nevét és értékeit. Minden entitás tartalmaznia kell a szükséges hello [PartitionKey és RowKey](#partitionkey-and-rowkey) tulajdonságok, továbbá tooany más tulajdonságokat megadhatja hello entitás.

Ez a példa létrehoz egy szótár objektumot képviselő entitás, majd továbbítja azokat toohello [insert_entity] [ py_insert_entity] metódus tooadd azt toohello tábla:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello trash', 'priority' : 200}
table_service.insert_entity('tasktable', task)
```

Ez a példa létrehoz egy [entitás] [ py_Entity] objektumot, majd továbbítja azokat toohello [insert_entity] [ py_insert_entity] metódus tooadd azt toohello tábla:

```python
task = Entity()
task.PartitionKey = 'tasksSeattle'
task.RowKey = '002'
task.description = 'Wash hello car'
task.priority = 100
table_service.insert_entity('tasktable', task)
```

### <a name="partitionkey-and-rowkey"></a>PartitionKey és RowKey

Meg kell adni egy **PartitionKey** és egy **RowKey** összes entitás tulajdonság. Ezek a hello egyedi azonosítói az entitások szerint együtt alkotják hello egy entitás elsődleges kulcsát. Ezek az értékek sokkal gyorsabb, mint bármely más entitás tulajdonságai, mivel csak ezek a Tulajdonságok indexelt kérdezheti használatával kérdezheti le.

Table szolgáltatás által használt hello **PartitionKey** toointelligently táblaentitásokat szét a tárhely csomópontot. Rendelkező entitásokat hello azonos **PartitionKey** hello tárolt ugyanahhoz a csomóponthoz. **RowKey** hello tartozik hello partíción belül hello entitás egyedi azonosítója.

## <a name="update-an-entity"></a>Egy entitás frissítése

minden tooupdate egy entitás tulajdonságértékek, hívja az hello [update_entity] [ py_update_entity] metódust. A példa bemutatja, hogyan tooreplace meglévő entitás frissített verzióra:

```python
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage', 'priority' : 250}
table_service.update_entity('tasktable', task)
```

Ha a frissítés alatt álló hello entitás nem létezik, majd hello frissítési művelet sikertelen lesz. Egy entitás toostore azt szeretné, hogy létezik-e, ha [insert_or_replace_entity][py_insert_or_replace_entity]. A következő példa hello első hívás hello lecseréli hello meglévő entitás. hello második hívás szúrja be egy új entitást, mivel nem entitás hello az adott PartitionKey és RowKey létezik hello táblában.

```python
# Replace hello entity created earlier
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '001', 'description' : 'Take out hello garbage again', 'priority' : 250}
table_service.insert_or_replace_entity('tasktable', task)

# Insert a new entity
task = {'PartitionKey': 'tasksSeattle', 'RowKey': '003', 'description' : 'Buy detergent', 'priority' : 300}
table_service.insert_or_replace_entity('tasktable', task)
```

> [!TIP]
> Hello [update_entity] [ py_update_entity] módszer a felváltja az összes tulajdonságok és értékek egy meglévő entitás, amely is használja a meglévő entitás tooremove tulajdonságait. Használhatja a hello [merge_entity] [ py_merge_entity] metódus tooupdate rendelkező új vagy módosított tulajdonságértékek nem teljesen cseréli le a hello entitás meglévő entitás.

## <a name="modify-multiple-entities"></a>Több entitás módosítása

tooensure kérelem atomi feldolgozási hello hello Table szolgáltatás által, elküldheti a több műveletei együtt egy kötegben. Először használja a hello [TableBatch] [ py_TableBatch] tooadd osztály több műveletek tooa kötegben. Ezután hívja [TableService][py_TableService].[ commit_batch] [ py_commit_batch] toosubmit hello műveletek atomi művelet. Kötegelt ugyanúgy módosított összes entitások toobe kell hello egyazon partícióra kerüljenek.

Ebben a példában két olyan entitásra kötegben együtt hozzáadása:

```python
from azure.storage.table import TableBatch
batch = TableBatch()
task004 = {'PartitionKey': 'tasksSeattle', 'RowKey': '004', 'description' : 'Go grocery shopping', 'priority' : 400}
task005 = {'PartitionKey': 'tasksSeattle', 'RowKey': '005', 'description' : 'Clean hello bathroom', 'priority' : 100}
batch.insert_entity(task004)
batch.insert_entity(task005)
table_service.commit_batch('tasktable', batch)
```

Kötegek hello környezetben szintaxisára együtt is használható:

```python
task006 = {'PartitionKey': 'tasksSeattle', 'RowKey': '006', 'description' : 'Go grocery shopping', 'priority' : 400}
task007 = {'PartitionKey': 'tasksSeattle', 'RowKey': '007', 'description' : 'Clean hello bathroom', 'priority' : 100}

with table_service.batch('tasktable') as batch:
    batch.insert_entity(task006)
    batch.insert_entity(task007)
```

## <a name="query-for-an-entity"></a>Egy entitás lekérdezése

egy entitás tooquery adja át a PartitionKey és RowKey toohello [TableService][py_TableService].[ get_entity] [ py_get_entity] metódust.

```python
task = table_service.get_entity('tasktable', 'tasksSeattle', '001')
print(task.description)
print(task.priority)
```

## <a name="query-a-set-of-entities"></a>Az entitások készletének lekérdezése

Az entitások készletének úgy, hogy megadja a szűrési karakterláncot a hello kérdezhetők **szűrő** paraméter. Ebben a példában megkeresi az összes feladat budapesti PartitionKey a szűrő alkalmazásával:

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'")
for task in tasks:
    print(task.description)
    print(task.priority)
```

## <a name="query-a-subset-of-entity-properties"></a>Az entitástulajdonságok egy részének lekérdezése

Minden entitás lekérdezésben visszaadja a tulajdonságok is korlátozhatja. Ezzel a módszerrel nevű *leképezése*, csökkenti a sávszélesség, és javíthatja a lekérdezési teljesítményt, különösen olyan nagyméretű az entitások vagy a eredményhalmazt. Használjon hello **válasszon** kívánt hello tulajdonságainak paraméter és pass hello nevében visszaadott toohello ügyfél.

a következő kód hello hello lekérdezést entitások csak hello leírását hello tábla adja vissza.

> [!NOTE]
> a következő kódrészletet works csak elleni hello Azure Storage hello. Hello storage emulator által nem támogatott.

```python
tasks = table_service.query_entities('tasktable', filter="PartitionKey eq 'tasksSeattle'", select='description')
for task in tasks:
    print(task.description)
```

## <a name="delete-an-entity"></a>Entitás törlése

Entitás törlése úgy, hogy a PartitionKey és RowKey toohello [delete_entity] [ py_delete_entity] metódust.

```python
table_service.delete_entity('tasktable', 'tasksSeattle', '001')
```

## <a name="delete-a-table"></a>Tábla törlése

Ha már nincs szüksége egy táblázatot vagy bármelyik hello entitások belül, hívja hello [delete_table] [ py_delete_table] metódus toopermanently hello tábla törlése az Azure Storage.

```python
table_service.delete_table('tasktable')
```

## <a name="next-steps"></a>Következő lépések

* [Az Azure Storage SDK for Python API-referencia](https://azure-storage.readthedocs.io/en/latest/index.html)
* [Az Azure Storage Python SDK](https://github.com/Azure/azure-storage-python)
* [Python fejlesztői központ](https://azure.microsoft.com/develop/python/)
* [A Microsoft Azure Tártallózó](../vs-azure-tools-storage-manage-with-storage-explorer.md): egy ingyenes, platformfüggetlen-alkalmazást a Windows, a macOS és a Linux vizuálisan adatok Azure Storage használata.

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
