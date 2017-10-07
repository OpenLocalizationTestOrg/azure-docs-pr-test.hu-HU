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
# <a name="invoke-stored-procedure-from-copy-activity-in-azure-data-factory"></a>A másolási tevékenység során az Azure Data Factory tárolt eljárás meghívása
Az adatok másolásakor [SQL Server](data-factory-sqlserver-connector.md) vagy [Azure SQL Database](data-factory-azure-sql-connector.md), konfigurálhatja a hello **SqlSink** a másolási tevékenység tooinvoke tárolt eljárást. Érdemes lehet toouse hello tárolt eljárás tooperform adatok beszúrása toohello céltáblázatban előtt minden további feldolgozás (egyesítés oszlopok, keresés, szúrás több táblák, stb.) szükséges. Ez a szolgáltatás kihasználja a [Table-Valued paraméterek](https://msdn.microsoft.com/library/bb675163.aspx). 

hello a következő példa bemutatja, hogyan tooinvoke tárolt eljárás egy olyan SQL Server-adatbázis a Data Factory-folyamathoz (másolási tevékenység):  

## <a name="output-dataset-json"></a>JSON kimeneti adatkészlet
Hello kimeneti adatkészlet JSON, állítson be hello **típus** való: **SqlServerTable**. Állítsa be úgy túl**AzureSqlTable** toouse egy Azure SQL-adatbázishoz. a következő hello **tableName** tulajdonságának meg kell egyeznie az első paraméter hello tárolt eljárás hello neve.  

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

## <a name="sqlsink-section-in-copy-activity-json"></a>A másolási tevékenység JSON SqlSink szakasz
Adja meg a hello **SqlSink** hello másolási tevékenység JSON szakasz az alábbiak szerint. tooinvoke során adatokat beszúrni hello fogadó vagy a cél adatbázis, a tárolt eljárás mindkét értékeket megadni **SqlWriterStoredProcedureName** és **SqlWriterTableType** tulajdonságait. Ezek a tulajdonságok leírását lásd: [SqlSink szakasz hello SQL Server-összekötő cikkben](data-factory-sqlserver-connector.md#sqlsink).

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

## <a name="stored-procedure-definition"></a>Tárolt eljárás meghatározása 
Az adatbázisban azonos nevet, amint hello hello tárolt eljárás megadása **SqlWriterStoredProcedureName**. hello tárolt eljárás bemeneti adatok forrás adattárból hello kezeli, és adatbeszúrást hello cél adatbázis egyik táblája. hello első paraméter a következő tárolt eljárás hello nevének meg kell egyeznie a hello adatkészlet JSON (Marketing) definiált hello táblanév.

```sql
CREATE PROCEDURE spOverwriteMarketing @Marketing [dbo].[MarketingType] READONLY, @stringData varchar(256)
AS
BEGIN
    DELETE FROM [dbo].[Marketing] where ProfileID = @stringData
    INSERT [dbo].[Marketing](ProfileID, State)
    SELECT * FROM @Marketing
END
```

## <a name="table-type-definition"></a>Tábla típusának megadása
Az adatbázis meghatározása hello táblatípus hello azonos nevet, amint a **SqlWriterTableType**. hello séma hello tábla típusú hello bemeneti adatkészlet sémája hello egyeznie kell.

```sql
CREATE TYPE [dbo].[MarketingType] AS TABLE(
    [ProfileID] [varchar](256) NOT NULL,
    [State] [varchar](256) NOT NULL
)
```

## <a name="next-steps"></a>Következő lépések
Tekintse át a következő összekötő cikket, amely a JSON-példák befejezéséhez hello: 

- [Azure SQL Database](data-factory-azure-sql-connector.md)
- [SQL Server](data-factory-sqlserver-connector.md)
