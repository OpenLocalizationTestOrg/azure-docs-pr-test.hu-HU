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
# <a name="map-source-dataset-columns-toodestination-dataset-columns"></a><span data-ttu-id="e4183-103">Forrás adatkészlet oszlopok toodestination dataset oszlop leképezése</span><span class="sxs-lookup"><span data-stu-id="e4183-103">Map source dataset columns toodestination dataset columns</span></span>
<span data-ttu-id="e4183-104">Oszlopleképezés hogyan megadott oszlopoknak megadott hello forrás tábla térkép toocolumns "structure" hello "structure" fogadó tábla használt toospecify lehet.</span><span class="sxs-lookup"><span data-stu-id="e4183-104">Column mapping can be used toospecify how columns specified in hello “structure” of source table map toocolumns specified in hello “structure” of sink table.</span></span> <span data-ttu-id="e4183-105">Hello **columnMapping** tulajdonság érhető el hello **typeProperties** hello másolási tevékenység szakasza.</span><span class="sxs-lookup"><span data-stu-id="e4183-105">hello **columnMapping** property is available in hello **typeProperties** section of hello Copy activity.</span></span>

<span data-ttu-id="e4183-106">Oszlop leképezése a következő forgatókönyvek hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="e4183-106">Column mapping supports hello following scenarios:</span></span>

* <span data-ttu-id="e4183-107">Minden oszlop hello forrás adatkészlet-szerkezetekben hello fogadó adatkészlet-szerkezetekben csatlakoztatott tooall oszlopok.</span><span class="sxs-lookup"><span data-stu-id="e4183-107">All columns in hello source dataset structure are mapped tooall columns in hello sink dataset structure.</span></span>
* <span data-ttu-id="e4183-108">Hello forrás adatkészlet-szerkezetekben hello oszlopok csoportja csatlakoztatott tooall oszlopok hello fogadó adatkészlet-szerkezetekben.</span><span class="sxs-lookup"><span data-stu-id="e4183-108">A subset of hello columns in hello source dataset structure is mapped tooall columns in hello sink dataset structure.</span></span>

<span data-ttu-id="e4183-109">Az alábbiakban hello hiba feltételek, amelyek kivétel:</span><span class="sxs-lookup"><span data-stu-id="e4183-109">hello following are error conditions that result in an exception:</span></span>

* <span data-ttu-id="e4183-110">Kevesebb oszlopot vagy több oszlop szerepel hello "structure" fogadó tábla mint hello leképezésben megadott.</span><span class="sxs-lookup"><span data-stu-id="e4183-110">Either fewer columns or more columns in hello “structure” of sink table than specified in hello mapping.</span></span>
* <span data-ttu-id="e4183-111">Ismétlődő leképezés.</span><span class="sxs-lookup"><span data-stu-id="e4183-111">Duplicate mapping.</span></span>
* <span data-ttu-id="e4183-112">SQL-lekérdezés eredménye nincs hello leképezésben megadott oszlop neve.</span><span class="sxs-lookup"><span data-stu-id="e4183-112">SQL query result does not have a column name that is specified in hello mapping.</span></span>

> [!NOTE]
> <span data-ttu-id="e4183-113">hello következő mintákat az Azure SQL és az Azure Blob, de alkalmazható tooany adattároló, amely támogatja a téglalap alakú adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="e4183-113">hello following samples are for Azure SQL and Azure Blob but are applicable tooany data store that supports rectangular datasets.</span></span> <span data-ttu-id="e4183-114">Állítsa be úgy a DataSet adatkészlet és a társított szolgáltatás definíciói példák toopoint toodata hello megfelelő adatforrás.</span><span class="sxs-lookup"><span data-stu-id="e4183-114">Adjust dataset and linked service definitions in examples toopoint toodata in hello relevant data source.</span></span>

## <a name="sample-1--column-mapping-from-azure-sql-tooazure-blob"></a><span data-ttu-id="e4183-115">Az Azure SQL-tooAzure blobból oszlopleképezés 1 – minta</span><span class="sxs-lookup"><span data-stu-id="e4183-115">Sample 1 – column mapping from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="e4183-116">Ez a példa hello bemeneti táblájának struktúrája, és az Azure SQL-adatbázis tooa SQL táblázat mutat.</span><span class="sxs-lookup"><span data-stu-id="e4183-116">In this sample, hello input table has a structure and it points tooa SQL table in an Azure SQL database.</span></span>

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

<span data-ttu-id="e4183-117">Ez a példa hello eredménytábla struktúrája, és az Azure blob Storage tárolóban tooa blob mutat.</span><span class="sxs-lookup"><span data-stu-id="e4183-117">In this sample, hello output table has a structure and it points tooa blob in an Azure blob storage.</span></span>

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

<span data-ttu-id="e4183-118">a következő JSON hello a másolási tevékenység során a folyamat határozza meg.</span><span class="sxs-lookup"><span data-stu-id="e4183-118">hello following JSON defines a copy activity in a pipeline.</span></span> <span data-ttu-id="e4183-119">hello oszlopok forrásból leképezve a fogadó toocolumns (**columnMappings**) hello segítségével **fordító** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="e4183-119">hello columns from source mapped toocolumns in sink (**columnMappings**) by using hello **Translator** property.</span></span>

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
<span data-ttu-id="e4183-120">**Oszlop-hozzárendelési folyamat:**</span><span class="sxs-lookup"><span data-stu-id="e4183-120">**Column mapping flow:**</span></span>

![Oszlop-hozzárendelési folyamat](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-tooazure-blob"></a><span data-ttu-id="e4183-122">Az Azure SQL-tooAzure blobból SQL-lekérdezés oszlopleképezés 2 – minta</span><span class="sxs-lookup"><span data-stu-id="e4183-122">Sample 2 – column mapping with SQL query from Azure SQL tooAzure blob</span></span>
<span data-ttu-id="e4183-123">Ez a példa egy SQL-lekérdezésben használt tooextract adatokat az Azure SQL hello táblanév, az oszlopnevek hello egyszerűen megadása "structure" szakaszban helyett.</span><span class="sxs-lookup"><span data-stu-id="e4183-123">In this sample, a SQL query is used tooextract data from Azure SQL instead of simply specifying hello table name and hello column names in “structure” section.</span></span> 

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
<span data-ttu-id="e4183-124">Ebben az esetben hello lekérdezés eredményei "structure" forrás megadott első csatlakoztatott toocolumns.</span><span class="sxs-lookup"><span data-stu-id="e4183-124">In this case, hello query results are first mapped toocolumns specified in “structure” of source.</span></span> <span data-ttu-id="e4183-125">A következő forrás "structure" hello oszlopok csatlakoztatott toocolumns a fogadó "structure" columnMappings megadott szabályait.</span><span class="sxs-lookup"><span data-stu-id="e4183-125">Next, hello columns from source “structure” are mapped toocolumns in sink “structure” with rules specified in columnMappings.</span></span>  <span data-ttu-id="e4183-126">Tegyük fel, hogy hello lekérdezés 5 oszlopok, két további oszlop, mint a "structure" forrás hello adja vissza.</span><span class="sxs-lookup"><span data-stu-id="e4183-126">Suppose hello query returns 5 columns, two more columns than those specified in hello “structure” of source.</span></span>

<span data-ttu-id="e4183-127">**Oszlop-hozzárendelési folyamat**</span><span class="sxs-lookup"><span data-stu-id="e4183-127">**Column mapping flow**</span></span>

![Oszlop leképezése adatfolyam-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a><span data-ttu-id="e4183-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e4183-129">Next steps</span></span>
<span data-ttu-id="e4183-130">A másolási tevékenység az oktatóanyag hello cikke:</span><span class="sxs-lookup"><span data-stu-id="e4183-130">See hello article for a tutorial on using Copy Activity:</span></span> 

- [<span data-ttu-id="e4183-131">Másolja az adatokat a Blob Storage tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="e4183-131">Copy data from Blob Storage tooSQL Database</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
