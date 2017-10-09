---
title: "használatával a Data Factory Amazon Redshift aaaMove adatait |} Microsoft Docs"
description: "További tudnivalók az Azure Data Factory használatával Amazon Redshift toomove adatok."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 01d15078-58dc-455c-9d9d-98fbdf4ea51e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jingwang
ms.openlocfilehash: 2a097320734ebdd57282d250f7fdba35741777f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-redshift-using-azure-data-factory"></a>Helyezze át az adatokat az Amazon Redshift Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello Azure Data Factory toomove adatait Amazon Redshift a másolási tevékenység. hello cikk épít, hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során. 

Amazon Redshift támogatott tooany fogadó adattár adatainak másolhatja. Adattároló mosdók hello másolási tevékenység által támogatott listájáért lásd: [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats). Adat-előállító jelenleg Amazon Redshift tooother adattárolókhoz, de nem az adatok áthelyezését más adatokat tároló tooAmazon Redshift áthelyezése adatokat.

## <a name="prerequisites"></a>Előfeltételek
* Ha adatok tooan helyszíni adattár helyez át, telepítse [az adatkezelési átjáró](data-factory-data-management-gateway.md) a helyi gépen. Ezt követően Grant adatkezelési átjáró (IP-cím használata hello gép) hello hozzáférés tooAmazon Redshift fürt. Lásd: [engedélyezés hozzáférés toohello fürt](http://docs.aws.amazon.com/redshift/latest/gsg/rs-gsg-authorize-cluster-access.html) utasításokat.
* Ha tooan az Azure data adattár, lásd: [Azure Data Center IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653) hello számítási IP-cím és hello Azure-adatközpont által használt SQL-tartományok.

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat Amazon Redshift forrásból származó különböző eszközök/API-k használatával hozhatja létre egy folyamatot.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md) hello másolása adatok varázslóval adatcsatorna létrehozásával gyors útmutatást.

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára. 

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello: 

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. 
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. 

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  Az adat-előállító entitások, amelyek az Amazon Redshift adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: [JSON-példa: adatok másolása az Amazon Redshift tooAzure Blob](#json-example-copy-data-from-amazon-redshift-to-azure-blob) című szakaszát. 

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAmazon Redshift részleteit tartalmazzák: 

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
a következő táblázat hello biztosít JSON-elemek adott tooAmazon Redshift kapcsolódó szolgáltatás leírását.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| type |hello type tulajdonságot kell beállítani: **AmazonRedshift**. |Igen |
| kiszolgáló |Kiszolgáló IP-címét vagy állomásnevét kiszolgálónevét hello Amazon Redshift. |Igen |
| port |Amazon Redshift server hello hello TCP port száma hello toolisten ügyfél-kommunikációhoz használ. |Nem, alapértelmezett érték: 5439 |
| adatbázis |Hello Amazon Redshift adatbázis neve. |Igen |
| felhasználónév |Hozzáférés toohello adatbázis-felhasználó nevét. |Igen |
| jelszó |Hello felhasználói fiókhoz tartozó jelszót. |Igen |

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
Szakaszok & meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (Azure SQL, az Azure blob, Azure-tábla, stb.).

Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú. Az adattár hello hello adatok hello helyét ismerteti. hello typeProperties szakasz típusú adatkészlet **RelationalTable** következő tulajdonságai hello (amely tartalmazza az Amazon Redshift dataset) rendelkezik.

| Tulajdonság | Leírás | Szükséges |
| --- | --- | --- |
| tableName |Hello tábla hello Amazon Redshift adatbázis, amelyre a társított szolgáltatás neve hivatkozik. |Nem (Ha **lekérdezés** a **RelationalSource** van megadva) |

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok & rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [létrehozása folyamatok](data-factory-create-pipelines.md) cikk. Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el.

Mivel tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. A másolási tevékenység során két érték források és mosdók hello típusától függően.

Ha másolási tevékenység forrása típusú **RelationalSource** (amely tartalmazza az Amazon Redshift), typeProperties szakaszában érhetők hello következő tulajdonságai:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| lekérdezés |Hello egyéni lekérdezés tooread adatok felhasználásával. |SQL-lekérdezési karakterlánc. Például: Válasszon * from tábla. |Nem (Ha **tableName** a **dataset** van megadva) |

## <a name="json-example-copy-data-from-amazon-redshift-tooazure-blob"></a>JSON-példa: adatok másolása az Amazon Redshift tooAzure Blob
Ez a példa bemutatja, hogyan toocopy adatait az Amazon Redshift adatbázis tooan Azure Blob Storage tárolóban. Azonban az adatok átmásolhatók **közvetlenül** közölt hello nyelő tooany [Itt](data-factory-data-movement-activities.md#supported-data-stores-and-formats) másolási tevékenység során az Azure Data Factory használatával hello.  

hello minta a következő data factory entitások hello rendelkezik:

* A társított szolgáltatás típusa [AmazonRedshift](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [RelationalTable](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [RelationalSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md##copy-activity-properties).

hello minta másol adatokat egy lekérdezés eredményét Amazon Redshift tooa BLOB minden órában. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

**Amazon Redshift társított szolgáltatáshoz:**

```json
{
    "name": "AmazonRedshiftLinkedService",
    "properties":
    {
        "type": "AmazonRedshift",
        "typeProperties":
        {
            "server": "< hello IP address or host name of hello Amazon Redshift server >",
            "port": <hello number of hello TCP port that hello Amazon Redshift server uses toolisten for client connections.>,
            "database": "<hello database name of hello Amazon Redshift database>",
            "username": "<username>",
            "password": "<password>"
        }
    }
}
```

**Az Azure tárolás társított szolgáltatásának:**

```json
{
  "name": "AzureStorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Amazon Redshift bemeneti adatkészlet:**

Beállítás `"external": true` hello Data Factory szolgáltatásnak tájékoztatja, hogy hello dataset külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák. Ez a tulajdonság tootrue be egy bemeneti adatkészlet nem hello feldolgozási soros tevékenység által létrehozott.

```json
{
    "name": "AmazonRedshiftInputDataset",
    "properties": {
        "type": "RelationalTable",
        "linkedServiceName": "AmazonRedshiftLinkedService",
        "typeProperties": {
            "tableName": "<Table name>"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

**Az Azure Blob kimeneti adatkészlet:**

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello kezdési ideje év, hónap, nap és óra részét használja.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazonredshift/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            },
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
            ]
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**A folyamat Azure Redshift (RelationalSource) és a fogadó Blob másolási tevékenység:**

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**RelationalSource** és **fogadó** típusuk értéke túl**BlobSink**. hello SQL-lekérdezésben megadott hello **lekérdezés** tulajdonság jelöli ki hello adatok hello toocopy óránként túlra.

```json
{
    "name": "CopyAmazonRedshiftToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "RelationalSource",
                        "query": "$$Text.Format('select * from MyTable where timestamp >= \\'{0:yyyy-MM-ddTHH:mm:ss}\\' AND timestamp < \\'{1:yyyy-MM-ddTHH:mm:ss}\\'', WindowStart, WindowEnd)"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonRedshiftInputDataset"
                    }
                ],
                "outputs": [
                    {
                        "name": "AzureBlobOutputDataSet"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1
                },
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "AmazonRedshiftToBlob"
            }
        ],
        "start": "2014-06-01T18:00:00Z",
        "end": "2014-06-01T19:00:00Z"
    }
}
```
### <a name="type-mapping-for-amazon-redshift"></a>Az Amazon Redshift leképezésének
A hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk másolási tevékenység hajt végre típusok toosink típusát Automatikus típusú konverzió a következő kétlépcsős megközelítést hello:

1. Natív típusok too.NET forrástípus konvertálása
2. .NET típusú toonative a fogadó típusa konvertálása

Adatok tooAmazon Redshift áthelyezésekor hello leképezéseket a következő Amazon Redshift típusok too.NET típusok használtak.

| Amazon Redshift típusa | .NET-alapú típusa |
| --- | --- |
| SMALLINT |Int16 |
| EGÉSZ SZÁM |Int32 |
| BIGINT |Int64 |
| DECIMÁLIS |Decimális |
| VALÓS |Egyetlen |
| A KÉTSZERES PONTOSSÁG |Dupla |
| LOGIKAI ÉRTÉK |Karakterlánc |
| KARAKTER |Karakterlánc |
| VARCHAR |Karakterlánc |
| DÁTUM |Dátum és idő |
| IDŐBÉLYEG |Dátum és idő |
| SZÖVEG |Karakterlánc |

## <a name="map-source-toosink-columns"></a>A forrásoszlopokat toosink leképezése
toolearn leképezési oszlopok az forrás adatkészlet toocolumns fogadó adatkészletben, lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="repeatable-read-from-relational-sources"></a>A relációs források ismételhető Olvasás
Amikor az adatok másolása relációs adattároló, tartsa ismételhetőség szem előtt tartva tooavoid nem kívánt eredmények. Az Azure Data Factoryben futtathatja a szelet manuálisan. Beállíthatja úgy is egy adatkészlet újrapróbálkozási házirendje, hogy a szelet akkor fut újra, ha hiba történik. A szelet akkor fut újra, vagy módon, ha van szüksége arról, hogy ugyanazokat az adatokat hello toomake hogyan olvasható függetlenül attól, hogy hányszor a szelet futtatása. Lásd: [Repeatable olvasni a relációs források](data-factory-repeatable-copy.md#repeatable-read-from-relational-sources)

## <a name="performance-and-tuning"></a>Teljesítmény- és hangolása
Lásd: [másolási tevékenység teljesítmény- és hangolása útmutató](data-factory-copy-activity-performance.md) kulcsról toolearn tényezők az adatátvitelt jelölik a (másolási tevékenység során) az Azure Data Factory és különböző módokon toooptimize hatás teljesítmény azt.

## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek hello:

* [Másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) való a másolási tevékenység során a folyamat létrehozásának lépéseit.
