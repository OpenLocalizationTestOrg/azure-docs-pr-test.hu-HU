---
title: az Azure Table aaaMove adatok |} Microsoft Docs
description: "Megtudhatja, hogyan toomove adatokat az Azure Table Storage Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 07b046b1-7884-4e57-a613-337292416319
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jingwang
ms.openlocfilehash: 3dc3da6d88854674a9108b600534bc5d07575f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-tooand-from-azure-table-using-azure-data-factory"></a>Adatok tooand áthelyezése az Azure Data Factory használatához Azure táblából
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatok Azure Table Storage és a másolási tevékenység. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során. 

Bármely támogatott forráshierarchiából adatokat tooAzure Table Storage tárolja, és az Azure Table Storage támogatott tooany fogadó adatok tárolásához adatainak másolhatja. Adatforrások vagy mosdók hello másolási tevékenység által támogatott adattárolókhoz listáját lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. 

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat az Azure Table Storage és a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  JSON-definíciók, amelyek használt toocopy adatokat az Azure Table Storage az adat-előállító entitások minták, lásd: [JSON példák](#json-examples) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Table Storage részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
Két különböző összekapcsolt szolgáltatások toolink egy Azure blob storage tooan az Azure data factory használatával. Ezek: **AzureStorage** társított szolgáltatás és **AzureStorageSas** társított szolgáltatás. hello Azure tárolás társított szolgáltatásának biztosít hello data Factory összetevőnek a globális hozzáférési toohello Azure Storage. Mivel hello Azure Storage SAS (közös hozzáférésű Jogosultságkód) kapcsolódó szolgáltatás biztosítja azt az Azure Storage korlátozott/időhöz kötött hozzáférés toohello hello adat-előállítóban. Nincsenek más különbségek a következő két összekapcsolt szolgáltatások között. Válassza ki az igényeinek megfelelő kapcsolódó hello szolgáltatást. hello következő szakaszokban további részleteket a következő két összekapcsolt szolgáltatások.

[!INCLUDE [data-factory-azure-storage-linked-services](../../includes/data-factory-azure-storage-linked-services.md)]

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

hello typeProperties szakasz más adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** típusú hello adatkészlet szakasz **AzureTable** hello alábbi tulajdonságokkal rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello hello Azure tábla adatbázispéldány táblájának, amelyre a társított szolgáltatás neve hivatkozik. |Igen. Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél. Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél. |

### <a name="schema-by-data-factory"></a>Adat-előállító sémája
Sémamentesadat-tárolókhoz, például az Azure tábla a Data Factory szolgáltatásnak hello kikövetkezteti hello séma a következő módokon hello egyikében:

1. Ha megadja az adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban hello Data Factory szolgáltatásnak tulajdonság eleget tegyen hello séma szerint ez a struktúra. Ebben az esetben ha egy sort tartalmaz egy olyan oszlop értékét, null értékű biztosított azt.
2. Ha nem ad meg adatok szerkezete hello hello segítségével **struktúra** hello adatkészlet-definícióban, adat-előállító tulajdonság hello séma kikövetkezteti hello adatok első sorának hello segítségével. Ebben az esetben ha hello első sor nem tartalmaz teljes séma hello, azokat az oszlopokat vannak nem talált a másolási művelet hello eredményét.

Sémamentes adatforrások hello célszerű ezért hello segítségével adatok szerkezete toospecify hello **struktúra** tulajdonság.

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti adatkészletek és házirendek érhetők el.

Tulajdonságok tevékenységek minden típusának hello hello tevékenységekre hello typeProperties szakaszában érhető el ugyanakkor függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

**AzureTableSource** következő tulajdonságai typeProperties szakaszban hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| azureTableSourceQuery |Hello egyéni lekérdezés tooread adatok felhasználásával. |Azure-tábla lekérdezési karakterlánc. Példák a következő szakaszban hello. |Nem. Egy Táblanév egy azureTableSourceQuery nélkül van megadva, a hello tábla összes rekordot másolt toohello cél. Ha egy azureTableSourceQuery is meg van adva, a rekordok hello táblázatból, amely eleget tesz a hello lekérdezés olyan másolt toohello cél. |
| azureTableSourceIgnoreTableNotFound |Azt jelzi, hogy swallow hello kivétel tábla nem létezik. |IGAZ<br/>HAMIS |Nem |

### <a name="azuretablesourcequery-examples"></a>azureTableSourceQuery példák
Ha Azure táblaoszlop karakterlánc típusú:

```JSON
azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', SliceStart)"
```

Ha Azure táblaoszlop dátum/idő típusú:

```JSON
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', SliceStart, SliceEnd)"
```

**AzureTableSink** következő tulajdonságai typeProperties szakaszban hello támogatja:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| azureTableDefaultPartitionKeyValue |Alapértelmezett partíció kulcsérték hello a fogadó által használható. |Egy karakterlánc-érték. |Nem |
| azureTablePartitionKeyName |Adja meg, amelynek értékeket fogja használni, mint partíciókulcsok hello oszlop neve. Ha nincs megadva, AzureTableDefaultPartitionKeyValue hello partíciókulcs lesz. |Egy oszlop neve. |Nem |
| azureTableRowKeyName |Adja meg, amelynek oszlop értékeit kulcsként sor hello oszlop neve. Ha nincs megadva, minden egyes sorára használjon a GUID Azonosítót. |Egy oszlop neve. |Nem |
| azureTableInsertType |hello mód tooinsert adatait az Azure táblájába.<br/><br/>Ez a tulajdonság szabja meg, hogy meglévő hello kimeneti táblát, amely megfelelő a partíció-és sorkulcsok sora cseréje vagy egyesített értékükre. <br/><br/>toolearn arról, hogyan működnek ezek a beállítások (lemezegyesítési és -csere), lásd: [Insert vagy az egyesítéses entitás](https://msdn.microsoft.com/library/azure/hh452241.aspx) és [Insert vagy az entitás cseréje](https://msdn.microsoft.com/library/azure/hh452242.aspx) témaköröket. <br/><br> Ez a beállítás érvényes szinten hello sor nem hello táblaszintű, és sem a lehetőség törli a nem létező hello bemeneti hello kimeneti tábla sorainak. |Egyesítés (alapértelmezett)<br/>cserélje le |Nem |
| WriteBatchSize |Amikor hello writeBatchSize vagy writeBatchTimeout találati adatok beillesztése hello Azure-tábla. |Egész szám (sorok száma) |Nem (alapértelmezett: 10000) |
| writeBatchTimeout |Adatbeszúrást hello Azure-tábla amikor hello writeBatchSize vagy writeBatchTimeout találati |A TimeSpan<br/><br/>Példa: "00: 20:00" (20 perc) |Nem (alapértelmezett toostorage ügyfél alapértelmezett időtúllépési érték 90 másodperc) |

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName
Képezze le a forrás oszlop tooa céloszlop hello fordító JSON tulajdonság használatával, mint hello azureTablePartitionKeyName hello céloszlop használatba vétele előtt.

A következő példa hello, a forrásoszlop DivisionID a csatlakoztatott toohello céloszlop: DivisionID.  

```JSON
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```
hello DivisionID hello partíciós kulcs van megadva.

```JSON
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID",
    "writeBatchSize": 100,
    "writeBatchTimeout": "01:00:00"
}
```
## <a name="json-examples"></a>JSON-példák
hello alábbi példák megadják minta JSON-definíciók használható toocreate folyamat használatával [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md) vagy [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md) vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Azok hogyan toocopy adatok tooand Azure Table Storage és az Azure Blob-adatbázis. Azonban az adatok átmásolhatók **közvetlenül** bármelyik hello források tooany hello támogatott nyelő. További információkért lásd: hello szakasz "támogatott adattárolókhoz és formátumok" a [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md).

## <a name="example-copy-data-from-azure-table-tooazure-blob"></a>Példa: Adatok másolása az Azure Table tooAzure Blob
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (blob & tábla használatos).
2. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureTable](#dataset-properties).
3. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
4. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [AzureTableSource](#activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másolja az adatokat az Azure Table tooa blob toohello alapértelmezett partícióhoz tartozó minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Az Azure tárolás társított szolgáltatásának:**

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
Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**. Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg. Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.  

**Az Azure tábla bemeneti adatkészlet:**

hello példa feltételezi, hogy létrehozott egy "MyTable" tábla Azure tábla.

"External" beállítása: "true" tájékoztatja hello Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "AzureTableInput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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

**Másolási tevékenység során a folyamat AzureTableSource és BlobSink:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**AzureTableSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott **AzureTableSourceQuery** tulajdonság kiválaszt hello adatok hello alapértelmezett partíció minden órában toocopy.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
            {
                "name": "AzureTabletoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                      {
                        "name": "AzureTableInput"
                    }
                ],
                "outputs": [
                      {
                            "name": "AzureBlobOutput"
                      }
                ],
                "typeProperties": {
                      "source": {
                        "type": "AzureTableSource",
                        "AzureTableSourceQuery": "PartitionKey eq 'DefaultPartitionKey'"
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

## <a name="example-copy-data-from-azure-blob-tooazure-table"></a>Példa: Adatok másolása az Azure Blob tooAzure tábla
a következő példa azt mutatja be hello:

1. A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties) (blob & tábla használatos)
2. Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
3. Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureTable](#dataset-properties).
4. Hello [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [AzureTableSink](#copy-activity-properties).

hello minta másol idősorozat adatokat az Azure blob tooan Azure-tábla óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**A társított szolgáltatásnak Azure storage (az Azure tábla & Blob):**

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

Az Azure Data Factory két típusú Azure Storage társított szolgáltatásokat támogat: **AzureStorage** és **AzureStorageSas**. Az első hello, hello kapcsolati karakterlánc, amely tartalmazza az hello fiókkulcs ad meg, és hello újabb egy, a közös hozzáférésű Jogosultságkód (SAS) Uri hello meg. Lásd: [összekapcsolt szolgáltatások](#linked-service-properties) című szakaszban talál információt.

**Az Azure Blob bemeneti adatkészletet:**

Adatok van felvett egy új blobból minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja év, hónap és nap részét hello kezdési ideje, valamint fájlnév hello kezdő időpontja óra részét hello. "external": "true" beállítás arról értesíti az adott hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
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

**Azure-tábla kimeneti adatkészlet:**

hello minta Azure Table "MyTable" nevű tooa adattábla másolja. Hozzon létre egy Azure tábla azonos számú oszlopot hello hello Blob CSV-fájl toocontain várt. Új sorok hozzáadásakor toohello tábla óránként.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

**Másolási tevékenység során a folyamat BlobSource és AzureTableSink:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**BlobSource** és **fogadó** típusuk értéke túl**AzureTableSink**.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline with copy activity",
    "activities":[  
      {
        "name": "AzureBlobtoTable",
        "description": "Copy Activity",
        "type": "Copy",
        "inputs": [
          {
            "name": "AzureBlobInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureTableOutput"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "BlobSource"
          },
          "sink": {
            "type": "AzureTableSink",
            "writeBatchSize": 100,
            "writeBatchTimeout": "01:00:00"
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
## <a name="type-mapping-for-azure-table"></a>Az Azure-tábla leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello.

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Áthelyezésekor adatok túl & Azure táblából hello következő [Azure Table szolgáltatás által meghatározott hozzárendelések](https://msdn.microsoft.com/library/azure/dd179338.aspx) használják az Azure tábla OData típusok too.NET típusánál, és ez fordítva is igaz.

| Az OData-adattípus | .NET-típusa | Részletek |
| --- | --- | --- |
| Edm.Binary |Byte] |Bájttömb too64 KB fel. |
| Edm.Boolean |logikai érték |Logikai érték. |
| Edm.DateTime |Dátum és idő |Egy 64 bites érték kifejezett, egyezményes világidő (UTC). hello támogatott dátum és idő tartomány kezdődik 12:00 éjféltől. január 1, i.. 1601. (SZ) (UTC). hello tartomány vége December 31 9999. |
| Edm.Double |Dupla |Egy 64 bites lebegőpontos értéket. |
| Edm.Guid |GUID |A 128 bites globálisan egyedi azonosítóját. |
| Edm.Int32 |Int32 |Egy 32 bites egész számot. |
| Edm.Int64 |Int64 |Egy 64 bites egész számot. |
| Edm.String |Karakterlánc |Az UTF-16 kódolású érték. Karakterlánc-értékek mentése too64 KB lehet. |

### <a name="type-conversion-sample"></a>Átalakítás minta
a következő minta hello szolgál az adatok másolása az Azure Blob tooAzure tábla a típuskonverziók.

Tegyük fel, hogy hello Blob-adathalmazra CSV formátumban van, és három oszlopot tartalmaz. Őket egyik hello hét napjára rövidített francia nevekkel egyéni dátum és idő formátumú dátum és idő oszlop.

Adja meg az alábbiak szerint együtt típusdefiníciók hello oszlopokhoz hello Blob-forrás adatkészlet.

```JSON
{
    "name": " AzureBlobInput",
    "properties":
    {
         "structure":
          [
                { "name": "userid", "type": "Int64"},
                { "name": "name", "type": "String"},
                { "name": "lastlogindate", "type": "Datetime", "culture": "fr-fr", "format": "ddd-MM-YYYY"}
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
        "external": true,
        "availability":
        {
            "frequency": "Hour",
            "interval": 1,
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
Megadott hello hozzárendelése az Azure tábla OData too.NET típusának, határozzák meg hello tábla Azure tábla a következő séma hello.

**Az Azure tábla sémája:**

| Oszlop neve | Típus |
| --- | --- |
| felhasználói azonosítóját |Edm.Int64 |
| név |Edm.String |
| lastlogindate |Edm.DateTime |

A következő határozza meg az alábbiak szerint hello Azure Table-adatkészlet. Mivel hello típusra vonatkozó adat már meg van adva az alapul szolgáló adattár hello nem kell toospecify "structure" szakasz hello típusú adatokkal.

```JSON
{
  "name": "AzureTableOutput",
  "properties": {
    "type": "AzureTable",
    "linkedServiceName": "StorageLinkedService",
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

Ebben az esetben Data Factory automatikusan írja be a átalakítások, beleértve az hello egyéni dátum és idő formátumban hello "fr-fr" kulturális környezet használatával, amikor adatokat Blob tooAzure tábla hello Datetime mező.

> [!NOTE]
> Tekintse meg a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
toolearn kulccsal kapcsolatos tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény, lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md).
