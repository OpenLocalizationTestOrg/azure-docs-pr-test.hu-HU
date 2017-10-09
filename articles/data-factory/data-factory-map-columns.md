---
title: Azure Data Factory aaaMapping dataset oszlopai |} Microsoft Docs
description: "Ismerje meg, hogyan toomap forrás oszlopok toodestination oszlopok."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8f78d4af675bec0a70e5f6e83ec1ffb511408b5a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a>Forrás adatkészlet oszlopok toodestination dataset oszlop leképezése
Oszlopleképezés hogyan megadott oszlopoknak megadott hello forrás tábla térkép toocolumns "structure" hello "structure" fogadó tábla használt toospecify lehet. Hello **columnMapping** tulajdonság érhető el hello **typeProperties** hello másolási tevékenység szakasza.

Oszlop leképezése a következő forgatókönyvek hello támogatja:

* Minden oszlop hello forrás adatkészlet-szerkezetekben hello fogadó adatkészlet-szerkezetekben csatlakoztatott tooall oszlopok.
* Hello forrás adatkészlet-szerkezetekben hello oszlopok csoportja csatlakoztatott tooall oszlopok hello fogadó adatkészlet-szerkezetekben.

Az alábbiakban hello hiba feltételek, amelyek kivétel:

* Kevesebb oszlopot vagy több oszlop szerepel hello "structure" fogadó tábla mint hello leképezésben megadott.
* Ismétlődő leképezés.
* SQL-lekérdezés eredménye nincs hello leképezésben megadott oszlop neve.

> [!NOTE]
> hello következő mintákat az Azure SQL és az Azure Blob, de alkalmazható tooany adattároló, amely támogatja a téglalap alakú adatkészletek. Állítsa be úgy a DataSet adatkészlet és a társított szolgáltatás definíciói példák toopoint toodata hello megfelelő adatforrás.

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a>Az Azure SQL-tooAzure blobból oszlopleképezés 1 – minta
Ez a példa hello bemeneti táblájának struktúrája, és az Azure SQL-adatbázis tooa SQL táblázat mutat.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

Ez a példa hello eredménytábla struktúrája, és az Azure blob Storage tárolóban tooa blob mutat.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

a következő JSON hello a másolási tevékenység során a folyamat határozza meg. hello oszlopok forrásból leképezve a fogadó toocolumns (**columnMappings**) hello segítségével **fordító** tulajdonság.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Oszlop-hozzárendelési folyamat:**

![Oszlop-hozzárendelési folyamat](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a>Az Azure SQL-tooAzure blobból SQL-lekérdezés oszlopleképezés 2 – minta
Ez a példa egy SQL-lekérdezésben használt tooextract adatokat az Azure SQL hello táblanév, az oszlopnevek hello egyszerűen megadása "structure" szakaszban helyett. 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
Ebben az esetben hello lekérdezés eredményei "structure" forrás megadott első csatlakoztatott toocolumns. A következő forrás "structure" hello oszlopok csatlakoztatott toocolumns a fogadó "structure" columnMappings megadott szabályait.  Tegyük fel, hogy hello lekérdezés 5 oszlopok, két további oszlop, mint a "structure" forrás hello adja vissza.

**Oszlop-hozzárendelési folyamat**

![Oszlop leképezése adatfolyam-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Következő lépések
A másolási tevékenység az oktatóanyag hello cikke: 

- [Másolja az adatokat a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
