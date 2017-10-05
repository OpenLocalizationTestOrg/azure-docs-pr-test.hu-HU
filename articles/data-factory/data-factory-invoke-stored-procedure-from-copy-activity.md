---
title: "Az Azure Data Factory másolási tevékenység tárolt eljárás meghívása |} Microsoft Docs"
description: "Megtudhatja, hogyan lehet meghívni egy Azure Data Factory másolási tevékenység során az Azure SQL Database vagy az SQL Server tárolt eljárást."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: af6e4a57e726598c266ee766656aa2cc22e374e3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a><span data-ttu-id="232fe-103">A másolási tevékenység során az Azure Data Factory tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="232fe-103">Invoke stored procedure from copy activity in Azure Data Factory</span></span>
<span data-ttu-id="232fe-104">Az adatok másolásakor [SQL Server](data-factory-sqlserver-connector.md) vagy [Azure SQL Database](data-factory-azure-sql-connector.md), konfigurálhatja a **SqlSink** a másolási tevékenység meghívni a tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="232fe-104">When copying data into [SQL Server](data-factory-sqlserver-connector.md) or [Azure SQL Database](data-factory-azure-sql-connector.md), you can configure the **SqlSink** in copy activity to invoke a stored procedure.</span></span> <span data-ttu-id="232fe-105">Érdemes lehet használni a tárolt eljárást a további feldolgozás (egyesítés oszlopok, keresés, szúrás több táblák, stb.) az adatok a céltáblázatba való beszúrása előtt meg kell adni.</span><span class="sxs-lookup"><span data-stu-id="232fe-105">You may want to use the stored procedure to perform any additional processing (merging columns, looking up values, insertion into multiple tables, etc.) is required before inserting data in to the destination table.</span></span> <span data-ttu-id="232fe-106">Ez a szolgáltatás kihasználja a [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb675163.aspx).</span><span class="sxs-lookup"><span data-stu-id="232fe-106">This feature takes advantage of [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).</span></span> 

<span data-ttu-id="232fe-107">A következő példa bemutatja, hogyan hívhatnak meg a Data Factory-folyamathoz (másolási tevékenység) SQL Server-adatbázisban tárolt eljárást:</span><span class="sxs-lookup"><span data-stu-id="232fe-107">The following sample shows how to invoke a stored procedure in a SQL Server database from a Data Factory pipeline (copy activity):</span></span>  

## <a name="output-dataset-json"></a><span data-ttu-id="232fe-108">JSON kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="232fe-108">Output dataset JSON</span></span>
<span data-ttu-id="232fe-109">A kimeneti adatkészlet JSON, állítsa be a **típus** való: **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="232fe-109">In the output dataset JSON, set the **type** to: **SqlServerTable**.</span></span> <span data-ttu-id="232fe-110">Állítsa az értékét **AzureSqlTable** Azure SQL-adatbázis használata.</span><span class="sxs-lookup"><span data-stu-id="232fe-110">Set it to **AzureSqlTable** to use with an Azure SQL database.</span></span> <span data-ttu-id="232fe-111">A következő **tableName** tulajdonságának meg kell egyeznie a tárolt eljárás első paraméter neve.</span><span class="sxs-lookup"><span data-stu-id="232fe-111">The value for **tableName** property must match the name of first parameter of the stored procedure.</span></span>  

```json
{
  "name": "SqlOutput",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlLinkedService",
    "typeProperties": {
      "tableName": "Marketing"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

## <a name="sqlsink-section-in-copy-activity-json"></a><span data-ttu-id="232fe-112">A másolási tevékenység JSON SqlSink szakasz</span><span class="sxs-lookup"><span data-stu-id="232fe-112">SqlSink section in copy activity JSON</span></span>
<span data-ttu-id="232fe-113">Adja meg a **SqlSink** a másolási tevékenység során JSON szakasz az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="232fe-113">Define the **SqlSink** section in the copy activity JSON as follows.</span></span> <span data-ttu-id="232fe-114">A tárolt eljárás hívása közben adatok beszúrása a fogadó vagy a cél-adatbázis, adja meg az értékeket is **SqlWriterStoredProcedureName** és **SqlWriterTableType** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="232fe-114">To invoke a stored procedure while inserting data into the sink/destination database, specify values for both **SqlWriterStoredProcedureName** and **SqlWriterTableType** properties.</span></span> <span data-ttu-id="232fe-115">Ezek a tulajdonságok leírását lásd: [SqlSink szakasz az SQL Server-összekötő cikkben](data-factory-sqlserver-connector.md#sqlsink).</span><span class="sxs-lookup"><span data-stu-id="232fe-115">For descriptions of these properties, see [SqlSink section in the SQL Server connector article](data-factory-sqlserver-connector.md#sqlsink).</span></span>

```json
"sink":
{
    "type": "SqlSink",
    "SqlWriterTableType": "MarketingType",
    "SqlWriterStoredProcedureName": "spOverwriteMarketing", 
    "storedProcedureParameters":
            {
                "stringData": 
                {
                    "value": "str1"     
                }
            }
}
```

## <a name="stored-procedure-definition"></a><span data-ttu-id="232fe-116">Tárolt eljárás meghatározása</span><span class="sxs-lookup"><span data-stu-id="232fe-116">Stored procedure definition</span></span> 
<span data-ttu-id="232fe-117">Az adatbázisban, adja meg a tárolt eljárás neve megegyezik a **SqlWriterStoredProcedureName**.</span><span class="sxs-lookup"><span data-stu-id="232fe-117">In your database, define the stored procedure with the same name as **SqlWriterStoredProcedureName**.</span></span> <span data-ttu-id="232fe-118">A tárolt eljárás kezeli a forrás adattárból bemeneti adatokat, és adatok beszúrása egy táblázatba a céladatbázisban.</span><span class="sxs-lookup"><span data-stu-id="232fe-118">The stored procedure handles input data from the source data store, and inserts data into a table in the destination database.</span></span> <span data-ttu-id="232fe-119">Az első paraméter a következő tárolt eljárás nevét meg kell egyeznie a tableName definiálva az adatkészlet JSON (Marketing).</span><span class="sxs-lookup"><span data-stu-id="232fe-119">The name of the first parameter of stored procedure must match the tableName defined in the dataset JSON (Marketing).</span></span>

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a><span data-ttu-id="232fe-120">Tábla típusának megadása</span><span class="sxs-lookup"><span data-stu-id="232fe-120">Table type definition</span></span>
<span data-ttu-id="232fe-121">Az adatbázisban azonos nevű tábla típusának azonosítására **SqlWriterTableType**.</span><span class="sxs-lookup"><span data-stu-id="232fe-121">In your database, define the table type with the same name as **SqlWriterTableType**.</span></span> <span data-ttu-id="232fe-122">A tábla típusú sémát, meg kell egyeznie a bemeneti adatkészlet sémája.</span><span class="sxs-lookup"><span data-stu-id="232fe-122">The schema of the table type must match the schema of the input dataset.</span></span>

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a><span data-ttu-id="232fe-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="232fe-123">Next steps</span></span>
<span data-ttu-id="232fe-124">Tekintse át a következő összekötő, amely annak teljes JSON-példák:</span><span class="sxs-lookup"><span data-stu-id="232fe-124">Review the following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="232fe-125">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="232fe-125">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="232fe-126">SQL Server</span><span class="sxs-lookup"><span data-stu-id="232fe-126">SQL Server</span></span>](data-factory-sqlserver-connector.md)
