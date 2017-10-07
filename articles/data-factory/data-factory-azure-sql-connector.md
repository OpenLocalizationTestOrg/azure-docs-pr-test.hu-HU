---
title: az Azure SQL Database aaaCopy adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatokat az Azure SQL adatbázis Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 484f735b-8464-40ba-a9fc-820e6553159e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: d2ff16191afb028da75699c5e4d0bb310538db0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-azure-sql-database-using-azure-data-factory"></a>Adatok tooand másolása az Azure SQL adatbázis Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok tooand az Azure SQL Database a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.  

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **az Azure SQL Database** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooAzure SQL-adatbázis**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

## <a name="supported-authentication-type"></a>Támogatott hitelesítési típushoz
Az Azure SQL Database-összekötő az egyszerű hitelesítést támogatja.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat belőle egy Azure SQL Database különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az Azure blob storage tooan Azure SQL-adatbázis, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure SQL adatbázis tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure SQL-adatbázis, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 
3. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát. És egy másik dataset toospecify hello SQL táblázat hello blob-tároló átmásolva hello adatokat tartalmazó hello Azure SQL-adatbázis létrehozása. Adatkészlet tulajdonságai, amelyek adott tooAzure Data Lake Store, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
4. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában BlobSource forrás-és SqlSink akár használhatja a fogadó hello másolási tevékenységhez. Hasonlóképpen a Blob Storage Azure SQL Database tooAzure másolása, használható SqlSource és BlobSink hello másolási tevékenység. A másolási tevékenység tulajdonságait, amelyek adott tooAzure SQL-adatbázis, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az Azure SQL adatbázis használt toocopy adatok JSON-definíciók minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-sql-database) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure SQL-adatbázis részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
Egy Azure SQL társított szolgáltatás hivatkozások egy Azure SQL adatbázis tooyour adat-előállítóban. a következő táblázat hello biztosít leírását a megadott JSON-elemek tooAzure SQL társított szolgáltatásnak.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **AzureSqlDatabase** |Igen |
| connectionString |Adjon meg információt hello connectionString tulajdonság szükséges tooconnect toohello Azure SQL Database-példányt. Csak az alapszintű hitelesítést is támogatja. |Igen |

> [!IMPORTANT]
> Konfigurálása [Azure SQL Database-tűzfal](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure) adatbázis-kiszolgáló túl hello[engedélyezése az Azure-szolgáltatások tooaccess hello server](https://msdn.microsoft.com/library/azure/ee621782.aspx#ConnectingFromAzure). Emellett külső Azure többek között a data factory átjáró a helyszíni adatforrásokból származó adatok tooAzure SQL-adatbázis másolása, beállítható, megfelelő IP-címtartomány hello gép küldő adatok tooAzure SQL-adatbázis.

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
a dataset toorepresent toospecify hello adatkészlet hello type tulajdonsága bemeneti vagy kimeneti adatok Azure SQL adatbázis beállítása: **AzureSqlTable**. Set hello **linkedServiceName** tulajdonság hello dataset toohello nevének hello Azure SQL társított szolgáltatásnak.  

Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** típusú hello adatkészlet szakasz **AzureSqlTable** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla vagy nézet hello Azure SQL Database-példányt, amelyre a társított szolgáltatás neve hivatkozik. |Igen |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

> [!NOTE]
> hello másolási tevékenység során csak egy bemenettel rendelkezik, és csak egy kimenetet.

Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha adatokat az Azure SQL-adatbázis, beállítása hello forrástípus hello másolási tevékenység túl**SqlSource**. Hasonlóképpen, ha az tooan Azure SQL-adatbázist, beállítása hello a fogadó típusa hello másolási tevékenység túl**SqlSink**. Ez a témakör SqlSource és SqlSink által támogatott tulajdonságokról.

### <a name="sqlsource"></a>SqlSource
A másolási tevékenység, ha hello adatforrás típusú **SqlSource**, hello a következő tulajdonságok érhetők el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| sqlReaderQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Példa: `select * from MyTable`. |Nem |
| sqlReaderStoredProcedureName |Hello neve tárolt eljárást, amely hello forrástábla olvassa be az adatokat. |Hello neve tárolt eljárást. hello utolsó SQL-utasítás hello tárolt eljárás SELECT utasítással kell lennie. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |

Ha hello **sqlReaderQuery** megadott hello SqlSource, hello másolási tevékenység során ez a lekérdezés futtatása hello Azure SQL Database forrás tooget hello adatok alapján. Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok-e a használt toobuild lekérdezés (`select column1, column2 from mytable`) toorun hello Azure SQL Database ellen. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

> [!NOTE]
> Használata esetén **sqlReaderStoredProcedureName**, továbbra is szükséges toospecify értéket hello **tableName** hello adatkészlet JSON tulajdonság. Nincs érvényesítést hajt végre ezt a táblázatot, ha van.
>
>

### <a name="sqlsource-example"></a>SqlSource – példa

```JSON
"source": {
    "type": "SqlSource",
    "sqlReaderStoredProcedureName": "CopyTestSrcStoredProcedureWithParameters",
    "storedProcedureParameters": {
        "stringData": { "value": "str3" },
        "identifier": { "value": "$$Text.Format('{0:yyyy}', SliceStart)", "type": "Int"}
    }
}
```

**hello tárolt eljárás definíciója:**

```SQL
CREATE PROCEDURE CopyTestSrcStoredProcedureWithParameters
(
    @stringData varchar(20),
    @identifier int
)
AS
SET NOCOUNT ON;
BEGIN
     select *
     from dbo.UnitTestSrcTable
     where dbo.UnitTestSrcTable.stringData != stringData
    and dbo.UnitTestSrcTable.identifier != identifier
END
GO
```

### <a name="sqlsink"></a>SqlSink
**SqlSink** következő tulajdonságai hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| writeBatchTimeout |Várnia kell az hello kötegelt beszúrási művelet toocomplete előtt azt az időkorlátot. |A TimeSpan<br/><br/> Példa: "00: 30:00" (30 perc). |Nem |
| WriteBatchSize |Amikor hello puffer mérete eléri writeBatchSize adatok beillesztése hello SQL táblázat. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| sqlWriterCleanupScript |Adja meg a másolási tevékenység tooexecute vonatkozó lekérdezést úgy, hogy egy adott szelet adatait. További információkért lásd: [ismételhető másolási](#repeatable-copy). |A lekérdezési utasítást. |Nem |
| sliceIdentifierColumnName |Adja meg oszlop nevét, a másolási tevékenység toofill szelet azonosító automatikusan létrejön, vagyis amikor futtassa újra a megadott szelet adatainak használt tooclean. További információkért lásd: [ismételhető másolási](#repeatable-copy). |Egy oszlop binary(32) adattípusú oszlop neve. |Nem |
| sqlWriterStoredProcedureName |Hello nevét (frissítés/Beszúrás) upserts adatok tárolt eljárás hello cél táblába. |Hello neve tárolt eljárást. |Nem |
| storedProcedureParameters |Hello paramétereinek tárolt eljárást. |A név/érték párok. Nevek és a kis-és paraméterek meg kell egyeznie hello nevét és a kis-és nagybetűhasználat hello tárolt eljárás paramétereit. |Nem |
| sqlWriterTableType |Adjon meg egy tábla Típus neve toobe hello tárolt eljárásban használt. Másolási tevékenység elérhetővé teszi hello adatok éppen áthelyezik egy ideiglenes táblát, amely a táblatípus. Tárolt eljárás kód majd egyesítheti a meglévő adatok másolásának hello adatokat. |Egy tábla környezettípus nevét. |Nem |

#### <a name="sqlsink-example"></a>SqlSink – példa

```JSON
"sink": {
    "type": "SqlSink",
    "writeBatchSize": 1000000,
    "writeBatchTimeout": "00:05:00",
    "sqlWriterStoredProcedureName": "CopyTestStoredProcedureWithParameters",
    "sqlWriterTableType": "CopyTestTableType",
    "storedProcedureParameters": {
        "identifier": { "value": "1", "type": "Int" },
        "stringData": { "value": "str1" },
        "decimalData": { "value": "1", "type": "Decimal" }
    }
}
```

## <a name="json-examples-for-copying-data-tooand-from-sql-database"></a>JSON Példák adatok tooand SQL-adatbázis másolása
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand az Azure SQL Database és az Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** bármelyik megadott hello nyelő források tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.

### <a name="example-copy-data-from-azure-sql-database-tooazure-blob"></a>Példa: Adatok másolása az Azure SQL Database tooAzure Blob
hello azonos meghatározza, hogy a következő adat-előállító entitások hello:

1. A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [Azure Blob](data-factory-azure-blob-connector.md#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL adatbázis tooa blob egy táblázatban minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.  

**A társított szolgáltatásnak Azure SQL Database:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Lásd: hello [Azure SQL társított szolgáltatást](#linked-service) hello lista szakasza a társított szolgáltatás által támogatott tulajdonságokról.

**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Lásd: hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében hello listát a társított szolgáltatás által támogatott tulajdonságokról.


**Az Azure SQL bemeneti adatkészlet:**

hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" Azure SQL, egy "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz.

"External" beállítása: "true" tájékoztatja hello Azure Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "AzureSqlInput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyTable"
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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

Lásd: hello [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza hello listáját.  

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}/",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
Lásd: hello [Azure-Blob adatkészletet típus tulajdonságainak](data-factory-azure-blob-connector.md#dataset-properties) hello lista szakasza ehhez az adathalmaztípushoz által támogatott tulajdonságokról.  

**A másolási tevékenység során az SQL-forrás és fogadó Blob egy folyamaton belül:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "AzureSQLtoBlob",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureSQLInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureBlobOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "BlobSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
     ]
   }
}
```
Hello példában **sqlReaderQuery** hello SqlSource van megadva. hello másolási tevékenység fut ez a lekérdezés hello Azure SQL Database forrásadatok tooget hello. Másik lehetőségként megadhat tárolt eljárás hello megadásával **sqlReaderStoredProcedureName** és **storedProcedureParameters** (ha hello tárolt eljárás paraméterek fogadja el).

Ha nem ad meg sqlReaderQuery vagy sqlReaderStoredProcedureName, hello adatkészlet JSON hello struktúra szakaszban meghatározott hello oszlopok használt toobuild egy lekérdezés toorun elleni hello Azure SQL Database. Például: `select column1, column2 from mytable`. Hello adatkészlet definíciója nem rendelkezik hello struktúra, ha minden kiválasztott oszlop. a hello táblából.

Lásd: hello [Sql-forrás](#sqlsource) szakasz és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSource és BlobSink által támogatott tulajdonságokról hello listáját.

### <a name="example-copy-data-from-azure-blob-tooazure-sql-database"></a>Példa: Adatok másolása az Azure Blob tooAzure SQL-adatbázis
hello minta meghatározza, hogy a következő adat-előállító entitások hello:  

1. A társított szolgáltatás típusa [AzureSqlDatabase](#linked-service-properties).
2. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
3. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSqlTable](#dataset-properties).
5. A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [SqlSink](#copy-activity-properties).

hello minta idősorozat adatainak másolása (óránként, naponta, stb.) az Azure SQL database az Azure blob tooa táblából óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Az Azure SQL társított szolgáltatásnak:**

```JSON
{
  "name": "AzureSqlLinkedService",
  "properties": {
    "type": "AzureSqlDatabase",
    "typeProperties": {
      "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=<databasename>;User ID=<username>@<servername>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
    }
  }
}
```
Lásd: hello [Azure SQL társított szolgáltatást](#linked-service) hello lista szakasza a társított szolgáltatás által támogatott tulajdonságokról.

**Az Azure Blob storage társított szolgáltatásnak:**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
Lásd: hello [Azure Blob](data-factory-azure-blob-connector.md#azure-storage-linked-service) cikkében hello listát a társított szolgáltatás által támogatott tulajdonságokról.


**Az Azure Blob bemeneti adatkészletet:**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az, hogy ez a táblázat külső toohello adat-előállító és hello adat-előállítóban tevékenység nem hozzák hello Data Factory szolgáltatásnak.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/",
      "fileName": "{Hour}.csv",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": "\n"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
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
Lásd: hello [Azure-Blob adatkészletet típus tulajdonságainak](data-factory-azure-blob-connector.md#dataset-properties) hello lista szakasza ehhez az adathalmaztípushoz által támogatott tulajdonságokról.

**Az Azure SQL Database kimeneti adatkészlet:**

hello minta másolja át a "MyTable" Azure SQL-nevű tooa adattábla. Hello tábla létrehozása az Azure SQL azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```JSON
{
  "name": "AzureSqlOutput",
  "properties": {
    "type": "AzureSqlTable",
    "linkedServiceName": "AzureSqlLinkedService",
    "typeProperties": {
      "tableName": "MyOutputTable"
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```
Lásd: hello [Azure SQL dataset típustulajdonságokat](#dataset) ehhez az adathalmaztípushoz által támogatott tulajdonságokról szakasza hello listáját.

**A másolási tevékenység során a Blob-forrás és fogadó SQL-feldolgozási folyamat:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**SqlSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoSQL",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSqlOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource",
            "blobColumnSeparators": ","
          },
          "sink": {
            "type": "SqlSink"
          }
        },
       "scheduler": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "OldestFirst",
          "retry": 0,
          "timeout": "01:00:00"
        }
      }
      ]
   }
}
```
Lásd: hello [Sql fogadó](#sqlsink) szakasz és [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) SqlSink és BlobSource által támogatott tulajdonságokról hello listáját.

## <a name="identity-columns-in-hello-target-database"></a>Azonosító oszlop hello céladatbázis
Ez a szakasz egy példát biztosít arra az adatok másolása a forrástábla egy azonosító oszlop tooa céltábla azonosító oszlopot tartalmazó nélkül.

**Forrástábla:**

```SQL
create table dbo.SourceTbl
(
       name varchar(100),
       age int
)
```
**Céltábla:**

```SQL
create table dbo.TargetTbl
(
       identifier int identity(1,1),
       name varchar(100),
       age int
)
```
Figyelje meg, hogy hello céltábla tartalmaz azonosító oszlopot.

**Forrás adatkészlet JSON-definícióból**

```JSON
{
    "name": "SampleSource",
    "properties": {
        "type": " SqlServerTable",
        "linkedServiceName": "TestIdentitySQL",
        "typeProperties": {
            "tableName": "SourceTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
```
**Cél adatkészlet JSON-definícióból**

```JSON
{
    "name": "SampleTarget",
    "properties": {
        "structure": [
            { "name": "name" },
            { "name": "age" }
        ],
        "type": "AzureSqlTable",
        "linkedServiceName": "TestIdentitySQLSource",
        "typeProperties": {
            "tableName": "TargetTbl"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": false,
        "policy": {}
    }    
}
```

Figyelje meg, hogy a forrás és cél táblázatként különböző sémája (cél rendelkezik egy olyan további oszlop identitású). Ebben az esetben szüksége toospecify **struktúra** hello tároló adatkészlet-definícióban, amely nem tartalmazza a hello azonosító oszlop tulajdonsága.

## <a name="invoke-stored-procedure-from-sql-sink"></a>A fogadó SQL tárolt eljárás meghívása
A folyamat a másolási tevékenység SQL fogadó egy tárolt eljárás végrehajtásával hívásának példáért lásd: [fogadó SQL tárolt eljárás meghívása a másolási tevékenység](data-factory-invoke-stored-procedure-from-copy-activity.md) cikk. 

## <a name="type-mapping-for-azure-sql-database"></a>Írja be az Azure SQL Database leképezése
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység az automatikus típuskonverziók származó típusok toosink típusait a 2. lépés – a módszert követve hello hajtja végre:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Ha adatok tooand tér át Azure SQL Database, hello következő megfeleltetéseket használ SQL too.NET típusának, és ez fordítva is igaz. hello ugyanaz, mint az SQL Server adattípus-hozzárendelése az ADO.NET hello lesz.

| SQL Server adatbázismotor típusa | .NET-keretrendszer típusa |
| --- | --- |
| bigint |Int64 |
| Bináris |Byte] |
| bit |Logikai érték |
| Karakter |Karakterlánc, Char] |
| Dátum |Dátum és idő |
| Dátum és idő |Dátum és idő |
| datetime2 |Dátum és idő |
| datetimeoffset |DateTimeOffset |
| Decimális |Decimális |
| A FILESTREAM attribútum (varbinary(max)) |Byte] |
| Lebegőpontos |Dupla |
| Kép |Byte] |
| int |Int32 |
| pénz |Decimális |
| nchar |Karakterlánc, Char] |
| ntext |Karakterlánc, Char] |
| Numerikus |Decimális |
| nvarchar |Karakterlánc, Char] |
| valós |Egyetlen |
| ROWVERSION |Byte] |
| smalldatetime |Dátum és idő |
| smallint |Int16 |
| kis pénz típusú értéknél |Decimális |
| sql_variant |Objektum * |
| Szöveg |Karakterlánc, Char] |
| time |A TimeSpan |
| időbélyeg |Byte] |
| tinyint |Bájt |
| egyedi azonosító |GUID |
| varbinary |Byte] |
| varchar |Karakterlánc, Char] |
| xml |XML |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-copy"></a>Ismételhető másolása
Adatok tooSQL Server-adatbázis másolásakor hello másolási tevékenység hozzáfűzi toohello fogadó adattábla alapértelmezés szerint. egy UPSERT tooperform helyett, lásd: [ismételhető írási tooSqlSink](data-factory-repeatable-copy.md#repeatable-write-to-sqlsink) cikk. 

Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [relációs források olvasni Repeatable](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.
