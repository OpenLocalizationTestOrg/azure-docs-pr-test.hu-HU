---
title: "aaaInvoke tárolt eljárást az Azure Data Factory másolási tevékenység |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinvoke Azure SQL Database vagy az Azure Data Factory az SQL Server tárolt eljárás másolási tevékenység."
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
ms.openlocfilehash: 986377118afb8c08607c2325fcc3ab00b3de9268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a><span data-ttu-id="6b037-103">A másolási tevékenység során az Azure Data Factory tárolt eljárás meghívása</span><span class="sxs-lookup"><span data-stu-id="6b037-103">Invoke stored procedure from copy activity in Azure Data Factory</span></span>
<span data-ttu-id="6b037-104">Az adatok másolásakor [SQL Server](data-factory-sqlserver-connector.md) vagy [Azure SQL Database](data-factory-azure-sql-connector.md), konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke tárolt eljárást.</span><span class="sxs-lookup"><span data-stu-id="6b037-104">When copying data into [SQL Server](data-factory-sqlserver-connector.md) or [Azure SQL Database](data-factory-azure-sql-connector.md), you can configure hello **SqlSink** in copy activity tooinvoke a stored procedure.</span></span> <span data-ttu-id="6b037-105">Érdemes lehet toouse hello tárolt eljárás tooperform adatok beszúrása toohello céltáblázatban előtt minden további feldolgozás (egyesítés oszlopok, keresés, szúrás több táblák, stb.) szükséges.</span><span class="sxs-lookup"><span data-stu-id="6b037-105">You may want toouse hello stored procedure tooperform any additional processing (merging columns, looking up values, insertion into multiple tables, etc.) is required before inserting data in toohello destination table.</span></span> <span data-ttu-id="6b037-106">Ez a szolgáltatás kihasználja a [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb675163.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b037-106">This feature takes advantage of [Table-Valued Parameters](https://msdn.microsoft.com/library/bb675163.aspx).</span></span> 

<span data-ttu-id="6b037-107">hello a következő példa bemutatja, hogyan tooinvoke tárolt eljárás egy olyan SQL Server-adatbázis a Data Factory-folyamathoz (másolási tevékenység):</span><span class="sxs-lookup"><span data-stu-id="6b037-107">hello following sample shows how tooinvoke a stored procedure in a SQL Server database from a Data Factory pipeline (copy activity):</span></span>  

## <a name="output-dataset-json"></a><span data-ttu-id="6b037-108">JSON kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="6b037-108">Output dataset JSON</span></span>
<span data-ttu-id="6b037-109">Hello kimeneti adatkészlet JSON, állítson be hello **típus** való: **SqlServerTable**.</span><span class="sxs-lookup"><span data-stu-id="6b037-109">In hello output dataset JSON, set hello **type** to: **SqlServerTable**.</span></span> <span data-ttu-id="6b037-110">Állítsa be úgy túl**AzureSqlTable** toouse egy Azure SQL-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="6b037-110">Set it too**AzureSqlTable** toouse with an Azure SQL database.</span></span> <span data-ttu-id="6b037-111">a következő hello **tableName** tulajdonságának meg kell egyeznie az első paraméter hello tárolt eljárás hello neve.</span><span class="sxs-lookup"><span data-stu-id="6b037-111">hello value for **tableName** property must match hello name of first parameter of hello stored procedure.</span></span>  

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

## <a name="sqlsink-section-in-copy-activity-json"></a><span data-ttu-id="6b037-112">A másolási tevékenység JSON SqlSink szakasz</span><span class="sxs-lookup"><span data-stu-id="6b037-112">SqlSink section in copy activity JSON</span></span>
<span data-ttu-id="6b037-113">Adja meg a hello **SqlSink** hello másolási tevékenység JSON szakasz az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="6b037-113">Define hello **SqlSink** section in hello copy activity JSON as follows.</span></span> <span data-ttu-id="6b037-114">tooinvoke során adatokat beszúrni hello fogadó vagy a cél adatbázis, a tárolt eljárás mindkét értékeket megadni **SqlWriterStoredProcedureName** és **SqlWriterTableType** tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="6b037-114">tooinvoke a stored procedure while inserting data into hello sink/destination database, specify values for both **SqlWriterStoredProcedureName** and **SqlWriterTableType** properties.</span></span> <span data-ttu-id="6b037-115">Ezek a tulajdonságok leírását lásd: [SqlSink szakasz hello SQL Server-összekötő cikkben](data-factory-sqlserver-connector.md#sqlsink).</span><span class="sxs-lookup"><span data-stu-id="6b037-115">For descriptions of these properties, see [SqlSink section in hello SQL Server connector article](data-factory-sqlserver-connector.md#sqlsink).</span></span>

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

## <a name="stored-procedure-definition"></a><span data-ttu-id="6b037-116">Tárolt eljárás meghatározása</span><span class="sxs-lookup"><span data-stu-id="6b037-116">Stored procedure definition</span></span> 
<span data-ttu-id="6b037-117">Az adatbázisban azonos nevet, amint hello hello tárolt eljárás megadása **SqlWriterStoredProcedureName**.</span><span class="sxs-lookup"><span data-stu-id="6b037-117">In your database, define hello stored procedure with hello same name as **SqlWriterStoredProcedureName**.</span></span> <span data-ttu-id="6b037-118">hello tárolt eljárás bemeneti adatok forrás adattárból hello kezeli, és adatbeszúrást hello cél adatbázis egyik táblája.</span><span class="sxs-lookup"><span data-stu-id="6b037-118">hello stored procedure handles input data from hello source data store, and inserts data into a table in hello destination database.</span></span> <span data-ttu-id="6b037-119">hello első paraméter a következő tárolt eljárás hello nevének meg kell egyeznie a hello adatkészlet JSON (Marketing) definiált hello táblanév.</span><span class="sxs-lookup"><span data-stu-id="6b037-119">hello name of hello first parameter of stored procedure must match hello tableName defined in hello dataset JSON (Marketing).</span></span>

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a><span data-ttu-id="6b037-120">Tábla típusának megadása</span><span class="sxs-lookup"><span data-stu-id="6b037-120">Table type definition</span></span>
<span data-ttu-id="6b037-121">Az adatbázis meghatározása hello táblatípus hello azonos nevet, amint a **SqlWriterTableType**.</span><span class="sxs-lookup"><span data-stu-id="6b037-121">In your database, define hello table type with hello same name as **SqlWriterTableType**.</span></span> <span data-ttu-id="6b037-122">hello séma hello tábla típusú hello bemeneti adatkészlet sémája hello egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="6b037-122">hello schema of hello table type must match hello schema of hello input dataset.</span></span>

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a><span data-ttu-id="6b037-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b037-123">Next steps</span></span>
<span data-ttu-id="6b037-124">Tekintse át a következő összekötő cikket, amely a JSON-példák befejezéséhez hello:</span><span class="sxs-lookup"><span data-stu-id="6b037-124">Review hello following connector articles that for complete JSON examples:</span></span> 

- [<span data-ttu-id="6b037-125">Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="6b037-125">Azure SQL Database</span></span>](data-factory-azure-sql-connector.md)
- [<span data-ttu-id="6b037-126">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6b037-126">SQL Server</span></span>](data-factory-sqlserver-connector.md)
