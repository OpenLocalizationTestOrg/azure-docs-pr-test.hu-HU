---
title: "Az Azure Data Factoryben az adatkészlet oszlopok leképezése |} Microsoft Docs"
description: "Megtudhatja, hogyan Forrásoszlopok leképezése cél oszlopok."
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
ms.openlocfilehash: a50661b377cfbbff3f1f762342cb275d5da82cea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a><span data-ttu-id="cff92-103">A dataset Forrásoszlopok leképezése cél adatkészlet oszlopok</span><span class="sxs-lookup"><span data-stu-id="cff92-103">Map source dataset columns to destination dataset columns</span></span>
<span data-ttu-id="cff92-104">Adja meg, hogyan megadott oszlopoknak a "szerkezetének" forrás tábla térkép oszlop szerepel a fogadó tábla "structure" oszlopleképezés is használható.</span><span class="sxs-lookup"><span data-stu-id="cff92-104">Column mapping can be used to specify how columns specified in the “structure” of source table map to columns specified in the “structure” of sink table.</span></span> <span data-ttu-id="cff92-105">A **columnMapping** tulajdonság érhető el a **typeProperties** a másolási tevékenység szakasza.</span><span class="sxs-lookup"><span data-stu-id="cff92-105">The **columnMapping** property is available in the **typeProperties** section of the Copy activity.</span></span>

<span data-ttu-id="cff92-106">Oszlop leképezése a következő szituációkat ismerteti:</span><span class="sxs-lookup"><span data-stu-id="cff92-106">Column mapping supports the following scenarios:</span></span>

* <span data-ttu-id="cff92-107">A forrás adatkészlet-szerkezetekben összes oszlop összes oszlop szerepel a fogadó adatkészlet-szerkezetekben van leképezve.</span><span class="sxs-lookup"><span data-stu-id="cff92-107">All columns in the source dataset structure are mapped to all columns in the sink dataset structure.</span></span>
* <span data-ttu-id="cff92-108">A forrás adatkészlet-szerkezetekben oszlopok egy részét az összes oszlop szerepel a fogadó adatkészlet-szerkezetekben van leképezve.</span><span class="sxs-lookup"><span data-stu-id="cff92-108">A subset of the columns in the source dataset structure is mapped to all columns in the sink dataset structure.</span></span>

<span data-ttu-id="cff92-109">Hiba feltételek, amelyek kivételt a következők:</span><span class="sxs-lookup"><span data-stu-id="cff92-109">The following are error conditions that result in an exception:</span></span>

* <span data-ttu-id="cff92-110">Kevesebb oszlopot vagy több oszlop szerepel a "structure" fogadó tábla mint a leképezésben megadott.</span><span class="sxs-lookup"><span data-stu-id="cff92-110">Either fewer columns or more columns in the “structure” of sink table than specified in the mapping.</span></span>
* <span data-ttu-id="cff92-111">Ismétlődő leképezés.</span><span class="sxs-lookup"><span data-stu-id="cff92-111">Duplicate mapping.</span></span>
* <span data-ttu-id="cff92-112">SQL-lekérdezés eredménye nem rendelkezik a leképezésben megadott oszlopnév.</span><span class="sxs-lookup"><span data-stu-id="cff92-112">SQL query result does not have a column name that is specified in the mapping.</span></span>

> [!NOTE]
> <span data-ttu-id="cff92-113">A következő mintákat az Azure SQL és az Azure Blob, de bármely adattároló, amely támogatja a téglalap alakú adatkészletek vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="cff92-113">The following samples are for Azure SQL and Azure Blob but are applicable to any data store that supports rectangular datasets.</span></span> <span data-ttu-id="cff92-114">Állítsa be úgy a DataSet adatkészlet és a társított szolgáltatás definíciók példákban az adatok a megfelelő adatforrás.</span><span class="sxs-lookup"><span data-stu-id="cff92-114">Adjust dataset and linked service definitions in examples to point to data in the relevant data source.</span></span>

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a><span data-ttu-id="cff92-115">Oszlopleképezés, az Azure SQL-ből az Azure-blobhoz 1 – minta</span><span class="sxs-lookup"><span data-stu-id="cff92-115">Sample 1 – column mapping from Azure SQL to Azure blob</span></span>
<span data-ttu-id="cff92-116">Ez a példa a bemeneti táblájának struktúrája, és egy Azure SQL adatbázis SQL táblázat mutat.</span><span class="sxs-lookup"><span data-stu-id="cff92-116">In this sample, the input table has a structure and it points to a SQL table in an Azure SQL database.</span></span>

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

<span data-ttu-id="cff92-117">Ez a példa a kimeneti táblához tartozik egy struktúra, és az Azure blob Storage tárolóban lévő blob mutat.</span><span class="sxs-lookup"><span data-stu-id="cff92-117">In this sample, the output table has a structure and it points to a blob in an Azure blob storage.</span></span>

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

<span data-ttu-id="cff92-118">A következő JSON egy folyamaton belül határozza meg a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="cff92-118">The following JSON defines a copy activity in a pipeline.</span></span> <span data-ttu-id="cff92-119">A forrás oszlop szerepel a fogadó leképezve oszlopokat (**columnMappings**) használatával a **fordító** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="cff92-119">The columns from source mapped to columns in sink (**columnMappings**) by using the **Translator** property.</span></span>

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
<span data-ttu-id="cff92-120">**Oszlop-hozzárendelési folyamat:**</span><span class="sxs-lookup"><span data-stu-id="cff92-120">**Column mapping flow:**</span></span>

![Oszlop-hozzárendelési folyamat](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a><span data-ttu-id="cff92-122">Az SQL-lekérdezést az Azure SQL-ből az Azure-blobhoz oszlopleképezés 2 – minta</span><span class="sxs-lookup"><span data-stu-id="cff92-122">Sample 2 – column mapping with SQL query from Azure SQL to Azure blob</span></span>
<span data-ttu-id="cff92-123">Ez a példa egy SQL-lekérdezés segítségével adatok kinyerése az Azure SQL helyett egyszerűen adja meg a táblanevet és az oszlop neve "structure" szakasz.</span><span class="sxs-lookup"><span data-stu-id="cff92-123">In this sample, a SQL query is used to extract data from Azure SQL instead of simply specifying the table name and the column names in “structure” section.</span></span> 

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
<span data-ttu-id="cff92-124">Ebben az esetben a lekérdezés eredményeinek először képezi le a "structure" forrás megadott oszlopok.</span><span class="sxs-lookup"><span data-stu-id="cff92-124">In this case, the query results are first mapped to columns specified in “structure” of source.</span></span> <span data-ttu-id="cff92-125">A következő forrás "structure" oszlopok vannak leképezve oszlop szerepel a fogadó "structure" columnMappings megadott szabályait.</span><span class="sxs-lookup"><span data-stu-id="cff92-125">Next, the columns from source “structure” are mapped to columns in sink “structure” with rules specified in columnMappings.</span></span>  <span data-ttu-id="cff92-126">Tegyük fel, hogy a lekérdezés 5 oszlopok, két további oszlop, mint a forrás "structure" adja vissza.</span><span class="sxs-lookup"><span data-stu-id="cff92-126">Suppose the query returns 5 columns, two more columns than those specified in the “structure” of source.</span></span>

<span data-ttu-id="cff92-127">**Oszlop-hozzárendelési folyamat**</span><span class="sxs-lookup"><span data-stu-id="cff92-127">**Column mapping flow**</span></span>

![Oszlop leképezése adatfolyam-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a><span data-ttu-id="cff92-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cff92-129">Next steps</span></span>
<span data-ttu-id="cff92-130">Olvassa el a másolási tevékenység az oktatóanyag:</span><span class="sxs-lookup"><span data-stu-id="cff92-130">See the article for a tutorial on using Copy Activity:</span></span> 

- [<span data-ttu-id="cff92-131">A Blob Storage az SQL Database-adatok másolása</span><span class="sxs-lookup"><span data-stu-id="cff92-131">Copy data from Blob Storage to SQL Database</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
