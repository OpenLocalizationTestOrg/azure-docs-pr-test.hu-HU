---
title: "aaaPush adatok tooSearch index Data Factory használatával |} Microsoft Docs"
description: "Megtudhatja, hogyan toopush adatok tooAzure Search-Index Azure Data Factory használatával."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: f8d46e1e-5c37-4408-80fb-c54be532a4ab
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/04/2017
ms.author: jingwang
ms.openlocfilehash: f2d973d0a2c24d6448e2d59e37e24503aa433018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="push-data-tooan-azure-search-index-by-using-azure-data-factory"></a>Adatok tooan Azure Search-index leküldéses Azure Data Factory használatával
Ez a cikk ismerteti, hogyan toouse hello másolási tevékenység toopush támogatott forrásadatok adattároló tooAzure Search-index. Támogatott forráshierarchiából adattárolókhoz hello forrásoszlopa hello szereplő [támogatott források és mosdók](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Ez a cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, amely adatmozgás általános áttekintést során másolási tevékenység és a támogatott adatokat tároló kombinációja.

## <a name="enabling-connectivity"></a>Kapcsolat engedélyezése
tooallow Data Factory szolgáltatásnak csatlakozás helyszíni adattár tooan, az adatkezelési átjáró telepítése a helyszíni környezetben. Átjáró telepíthető hello ugyanaz a gép, amelyen hello forrásadatok tárolja, vagy egy másik számítógépre tooavoid hello adatokkal erőforrások használják a tárolja.

Az adatkezelési átjáró csatlakozik a helyszíni adatok források toocloud szolgáltatások biztonságos és felügyelt módon. Lásd: [helyezze át az adatokat a helyszíni és a felhő között](data-factory-move-data-between-onprem-and-cloud.md) szóló cikkben olvashat az adatkezelési átjáró.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely a leküldéses értesítések adatok egy forrás adatokat tároló tooAzure Search-index a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek használt toocopy adatok tooAzure Search-index JSON-definíciók minta, lásd: [JSON-példa: adatok másolása a helyszíni SQL Server tooAzure Search-index](#json-example-copy-data-from-on-premises-sql-server-to-azure-search-index) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAzure Search-Index részleteit tartalmazzák:

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok

hello a következő táblázat ismerteti, amelyek adott toohello csatolt Azure Search szolgáltatás JSON-elemek szerepelnek.

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| type | hello type tulajdonságot kell beállítani: **AzureSearch**. | Igen |
| URL-címe | Hello Azure Search szolgáltatás URL-címe. | Igen |
| kulcs | Adminisztrátori kulcsot a hello Azure Search szolgáltatás. | Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai

Illetve meghatározásához adatkészletek rendelkezésre álló tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend a DataSet adatkészlet JSON hasonlítanak minden adatkészlet esetében. Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú. a DataSet hello típusú szakasz hello typeProperties **AzureSearchIndex** rendelkezik hello következő tulajdonságai:

| Tulajdonság | Leírás | Szükséges |
| -------- | ----------- | -------- |
| type | hello type tulajdonság túl be kell állítani**AzureSearchIndex**.| Igen |
| indexName | Hello Azure Search-index neve. Adat-előállító nem hoz létre hello index. az Azure Search léteznie kell a hello index. | Igen |


## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok és tevékenységek meghatározásához rendelkezésre álló tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, bemeneti és kimeneti tábláinak és különböző házirendek tulajdonságok minden típusú tevékenységek érhetők el. Mivel a hello typeProperties szakaszban rendelkezésre álló tulajdonságok tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

A másolási tevékenység, ha hello fogadó hello típusú **AzureSearchIndexSink**, typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| -------- | ----------- | -------------- | -------- |
| WriteBehavior | Meghatározza, hogy toomerge vagy cserélje le a dokumentum már létezik hello index. Lásd: hello [WriteBehavior tulajdonság](#writebehavior-property).| Egyesítés (alapértelmezett)<br/>Feltöltés| Nem |
| WriteBatchSize | Fájlfeltöltések hello Azure Search-index az adatokat, amikor hello puffer mérete eléri writeBatchSize. Lásd: hello [WriteBatchSize tulajdonság](#writebatchsize-property) részleteiről. | 1 too1 000. Alapértelmezett érték 1000. | Nem |

### <a name="writebehavior-property"></a>WriteBehavior tulajdonság
AzureSearchSink upserts adatok írásakor. Más szóval történő írásakor egy dokumentumot, ha hello dokumentum kulcs már létezik a hello Azure Search-index, az Azure Search frissíti hello meglévő dokumentumról, hanem egy ütközés Kivétel kiváltása.

hello AzureSearchSink két upsert viselkedések (AzureSearch SDK használatával) a következő hello biztosítja:

- **Egyesítési**: hello új dokumentum hello oszlopok egyesíthető egy meglévő hello. Az új dokumentum hello null értékű oszlopokhoz hello értéket egy meglévő hello megőrződik.
- **Töltse fel**: hello új dokumentum cserél hello meglévőt. Nincs megadva a hello új dokumentum oszlopok hello értéke toonull van-e egy nem null értéket hello meglévő dokumentum vagy sem.

hello alapértelmezett viselkedése **egyesítése**.

### <a name="writebatchsize-property"></a>WriteBatchSize tulajdonság
Az Azure Search szolgáltatás egy kötegelt dokumentumok írása támogatja. A kötegelt 1 too1, 000 műveletek is tartalmazhat. Egy művelet egy dokumentum tooperform hello feltöltés/egyesítési művelet kezeli.

### <a name="data-type-support"></a>Adattípus-támogatás
hello alábbi táblázat megadja, hogy egy Azure Search adattípus támogatott-e, vagy nem.

| Az Azure Search-adattípus | Az Azure Search fogadó támogatott |
| ---------------------- | ------------------------------ |
| Karakterlánc | I |
| Int32 | I |
| Int64 | I |
| Dupla | I |
| Logikai érték | I |
| DataTimeOffset | I |
| Karakterlánc-tömbben | N |
| GeographyPoint | N |

## <a name="json-example-copy-data-from-on-premises-sql-server-tooazure-search-index"></a>JSON-példa: adatok másolása a helyszíni SQL Server tooAzure Search-index

a következő példa azt mutatja be hello:

1.  A társított szolgáltatás típusa [AzureSearch](#linked-service-properties).
2.  A társított szolgáltatás típusa [OnPremisesSqlServer](data-factory-sqlserver-connector.md#linked-service-properties).
3.  Bemeneti [dataset](data-factory-create-datasets.md) típusú [SqlServerTable](data-factory-sqlserver-connector.md#dataset-properties).
4.  Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureSearchIndex](#dataset-properties).
4.  A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [SqlSource](data-factory-sqlserver-connector.md#copy-activity-properties) és [AzureSearchIndexSink](#copy-activity-properties).

hello minta másol idősorozat adatokat a helyszíni SQL Server adatbázis tooan Azure Search-index óránként. Ez a minta használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

Első lépésként a telepítő hello az adatkezelési átjáró a helyi számítógépen. hello utasítások szerepelnek hello [adatokat a helyszíni helyek és a felhő közötti áthelyezése](data-factory-move-data-between-onprem-and-cloud.md) cikk.

**Az Azure Search kapcsolódó szolgáltatás:**

```JSON
{
    "name": "AzureSearchLinkedService",
    "properties": {
        "type": "AzureSearch",
        "typeProperties": {
            "url": "https://<service>.search.windows.net",
            "key": "<AdminKey>"
        }
    }
}
```

**Kapcsolódó SQL Server szolgáltatás**

```JSON
{
  "Name": "SqlServerLinkedService",
  "properties": {
    "type": "OnPremisesSqlServer",
    "typeProperties": {
      "connectionString": "Data Source=<servername>;Initial Catalog=<databasename>;Integrated Security=False;User ID=<username>;Password=<password>;",
      "gatewayName": "<gatewayname>"
    }
  }
}
```

**SQL Server bemeneti adatkészlet**

hello minta azt feltételezi, hogy létrehozott egy tábla "MyTable" SQL Server és a "timestampcolumn" nevű adatsorozat időadatok oszlopot tartalmaz. Több tábla belül azonos adatbázist egyetlen dataset, de egy táblát kell használni az hello dataset tableName typeProperty hello keresztül kérdezheti le.

"External" beállítása: "true" tájékoztatja Data Factory szolgáltatásnak, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák.

```JSON
{
  "name": "SqlServerDataset",
  "properties": {
    "type": "SqlServerTable",
    "linkedServiceName": "SqlServerLinkedService",
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

**Az Azure Search kimeneti adatkészlet:**

hello minta másolatok adatok tooan Azure Search-index nevű **termékek**. Adat-előállító nem hoz létre hello index. tootest hello mintát, index létrehozása ezen a néven. Hozzon létre hello Azure Search-index hello azonos számú oszlopot hasonlóan hello bemeneti adatkészletet. Új bejegyzések kerülnek toohello Azure Search-index óránként.

```JSON
{
    "name": "AzureSearchIndexDataset",
    "properties": {
        "type": "AzureSearchIndex",
        "linkedServiceName": "AzureSearchLinkedService",
        "typeProperties" : {
            "indexName": "products",
        },
        "availability": {
            "frequency": "Minute",
            "interval": 15
        }
   }
}
```

**Másolási tevékenység során a folyamat az SQL-forrás és fogadó Azure Search-Index:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**SqlSource** és **fogadó** típusuk értéke túl**AzureSearchIndexSink**. hello SQL-lekérdezésben megadott hello **SqlReaderQuery** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```JSON
{  
    "name":"SamplePipeline",
    "properties":{  
    "start":"2014-06-01T18:00:00",
    "end":"2014-06-01T19:00:00",
    "description":"pipeline for copy activity",
    "activities":[  
      {
        "name": "SqlServertoAzureSearchIndex",
        "description": "copy activity",
        "type": "Copy",
        "inputs": [
          {
            "name": " SqlServerInput"
          }
        ],
        "outputs": [
          {
            "name": "AzureSearchIndexDataset"
          }
        ],
        "typeProperties": {
          "source": {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('select * from MyTable where timestampcolumn >= \\'{0:yyyy-MM-dd HH:mm}\\' AND timestampcolumn < \\'{1:yyyy-MM-dd HH:mm}\\'', WindowStart, WindowEnd)"
          },
          "sink": {
            "type": "AzureSearchIndexSink"
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

Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező. hello JSON alábbi kódrészletben láthatja a másolási tevékenység során szükséges hello módosítása `typeProperties` példaként. Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```


## <a name="copy-from-a-cloud-source"></a>A felhő forrás másolása
Ha a másolt adatok egy felhőalapú adattárból Azure Search szolgáltatásba történő `executionLocation` tulajdonság megadása kötelező. hello JSON alábbi kódrészletben láthatja a másolási tevékenység során szükséges hello módosítása `typeProperties` példaként. Ellenőrizze [felhőalapú adattároló közötti másolásához](data-factory-data-movement-activities.md#global) szakaszban a támogatott értékek és a további részleteket.

```JSON
"typeProperties": {
  "source": {
    "type": "BlobSource"
  },
  "sink": {
    "type": "AzureSearchIndexSink"
  },
  "executionLocation": "West US"
}
```

Forrás adatkészlet toocolumns hello másolási tevékenységdefinícióban fogadó adatkészletből oszlopokat is leképezheti. További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás  
Lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn hatás teljesítmény adatátvitelt jelölik a (másolási tevékenység), és különböző módokon toooptimize tényezők azt.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek hello:

* [Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.
