---
title: "aaaMove Data Factory használatával Amazon egyszerű tárolási szolgáltatás adatait |} Microsoft Docs"
description: "Megtudhatja, hogyan toomove Azure Data Factory használatával Amazon egyszerű tárolási szolgáltatás (S3) adatait."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 636d3179-eba8-4841-bcb4-3563f6822a26
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/20/2017
ms.author: jingwang
ms.openlocfilehash: 8a8cd2845fd1de74413bd0372f3aabfb4817549b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-data-from-amazon-simple-storage-service-by-using-azure-data-factory"></a>Adatok áthelyezése tárolószolgáltatásból Amazon egyszerű Azure Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse hello másolási tevékenység az Azure Data Factory toomove adatok tárolószolgáltatásból Amazon egyszerű (S3). -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikket, amely adatmozgás általános áttekintést hello másolási tevékenység során.

Amazon S3 támogatott tooany fogadó adattár adatainak másolhatja. Az adatok támogatott tárolja, a fogadók esetében hello másolási tevékenység, lásd: hello [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) tábla. Adat-előállító jelenleg támogatja a mozgóátlag adatok kizárólag az Amazon S3 tooother adattárolókhoz, de tooAmazon S3 adatok nem áthelyezését más adatokat tárolja.

## <a name="required-permissions"></a>Szükséges engedélyek
toocopy adatait Amazon S3, ellenőrizze, hogy rendelkezik a következő engedélyek hello:

* `s3:GetObject`és `s3:GetObjectVersion` Amazon S3 objektum műveletekhez.
* `s3:ListBucket`Amazon S3 gyűjtő műveletekhez. Hello Data Factory másolása varázsló használata `s3:ListAllMyBuckets` is szükség.

További információk az Amazon S3 engedélyek hello teljes listája: [megadása engedélyeket egy házirendben](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).

## <a name="getting-started"></a>Bevezetés
A másolási tevékenység, amely Amazon S3 forrásból származó adatokat a különböző eszközök vagy API-k használatával helyezi át a feldolgozási sor hozhatja létre.

hello legegyszerűbb módja toocreate adatcsatorna toouse hello **másolása varázsló**. Gyors bemutatóért lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Részletes útmutatás toocreate a másolási tevékenység során a folyamat, lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Akár eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban.
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet.
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel.

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök vagy API-k (kivéve a .NET API-t) használ, akkor határozzák meg a Data Factory entitások hello JSON formátumban. Az adat-előállító entitások, amelyek az Amazon S3 adattárolóból használt toocopy adatok JSON-definíciók minta, lásd: hello [JSON-példa: adatok másolása az Amazon S3 tooAzure Blob](#json-example-copy-data-from-amazon-s3-to-azure-blob) című szakaszát.

> [!NOTE]
> A másolási tevékenység során támogatott fájl- és tömörítési formátumok kapcsolatos részletekért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md).

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooAmazon S3 részleteit tartalmazzák.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz. Típusú társított szolgáltatás létrehozása **AwsAccessKey** toolink az Amazon S3 adatok tárolásához tooyour adat-előállítóban. a következő táblázat hello leírását adott JSON-elemek tooAmazon S3 (AwsAccessKey) társított szolgáltatást biztosít.

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| accessKeyID |Hello titkos hívóbetű azonosítója. |Karakterlánc |Igen |
| secretAccessKey |hello titkos hívóbetű magát. |Titkosított titkos karakterlánc |Igen |

Például:

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
a dataset toorepresent toospecify bemeneti adatai az Azure Blob storage hello típus tulajdonságbeállító hello adatkészlet túl**AmazonS3**. Set hello **linkedServiceName** toohello adatkészletnevet hello az Amazon S3 hello tulajdonságának társított szolgáltatás. Szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: [adatkészletek létrehozása](data-factory-create-datasets.md). 

Például struktúra, a rendelkezésre állás és a házirend hasonlítanak minden adatkészlet esetében (például az SQL-adatbázis, az Azure blob és az Azure tábla). Hello **typeProperties** szakasz eltérő adatkészlet egyes típusai és hello adattár hello adatok hello helyét ismerteti. Hello **typeProperties** szakasz egy adatkészlet típusú **AmazonS3** következő tulajdonságai hello (amely tartalmazza a hello Amazon S3 dataset) rendelkezik:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| bucketName |hello S3 gyűjtő neve. |Karakterlánc |Igen |
| kulcs |hello S3 objektum kulcsának. |Karakterlánc |Nem |
| előtag |Hello S3 Objektumkulcs előtagját. Kiválasztott objektumok, amelynek kulcsait a előtaggal kezdődik. Érvényes, csak ha kulcsa üres. |Karakterlánc |Nem |
| Verzió |hello S3 objektum, ha engedélyezve van a S3 versioning hello verziója. |Karakterlánc |Nem |
| Formátumban | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**,  **ParquetFormat**. Set hello **típus** tulajdonság alapján formátum tooone ezeket az értékeket. További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [Orc formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) szakaszok. <br><br> Ha azt szeretné, hogy toocopy fájlokat-fájlalapú tárolók (bináris másolás), skip hello formátum szakasz mindkét bemeneti és kimeneti adatkészlet definíciók között. |Nem | |
| Tömörítés | Adja meg a hello típusát és hello adatok tömörítése szintjét. hello támogatott típusok a következők: **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. hello támogatott szintek a következők: **Optimal** és **leggyorsabb**. További információkért lásd: [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem | |


> [!NOTE]
> **bucketName + kulcs** hello S3 objektum, amelyen gyűjtő S3 objektumok hello legfelső szintű tárolója, és kulcs hello teljes elérési útja toohello S3 objektum hello helyet határoz meg.

### <a name="sample-dataset-with-prefix"></a>Minta-adatkészleteken előtaggal

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "prefix": "testFolder/test",
            "bucketName": "testbucket",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```
### <a name="sample-dataset-with-version"></a>Minta-adatkészleteken (verziójával)

```json
{
    "name": "dataset-s3",
    "properties": {
        "type": "AmazonS3",
        "linkedServiceName": "link- testS3",
        "typeProperties": {
            "key": "testFolder/test.orc",
            "bucketName": "testbucket",
            "version": "XXXXXXXXXczm0CJajYkHf0_k6LhBmkcL",
            "format": {
                "type": "OrcFormat"
            }
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true
    }
}
```

### <a name="dynamic-paths-for-s3"></a>S3 dinamikus útvonalak
hello előző példa értékeket használ a rögzített hello **kulcs** és **bucketName** hello Amazon S3 adatkészlet tulajdonságai.

```json
"key": "testFolder/test.orc",
"bucketName": "testbucket",
```

Adat-előállító kiszámításához ezeket a tulajdonságokat, dinamikusan futásidőben, például a SliceStart rendszerváltozók használatával lehet.

```json
"key": "$$Text.Format('{0:MM}/{0:dd}/test.orc', SliceStart)"
"bucketName": "$$Text.Format('{0:yyyy}', SliceStart)"
```

Mindent hello azonos hello **előtag** az Amazon S3 adatkészlet tulajdonság. Támogatott funkciók és változók listájáért lásd: [adat-előállító funkciók és rendszerváltozók](data-factory-functions-variables.md).

## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: [folyamatok létrehozása](data-factory-create-pipelines.md). Az összes tevékenység tulajdonságai, például nevét, leírását, valamint bemeneti és kimeneti táblák és házirendek érhetők el. Tulajdonságok érhetők el hello **typeProperties** hello tevékenység szakasza tevékenységek minden típusának függenek. Hello másolási tevékenységhez tulajdonságok hello típusú források és mosdók függenek. Ha egy forrást a hello másolási tevékenység típusa nem **FileSystemSource** (amely tartalmazza az Amazon S3), a következő tulajdonság hello érhető el **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| Rekurzív |Meghatározza, hogy toorecursively lista S3 objektumokat hello könyvtára alatt tárolja. |Igaz/hamis |Nem |

## <a name="json-example-copy-data-from-amazon-s3-tooazure-blob-storage"></a>JSON-példa: adatok másolása az Amazon S3 tooAzure Blob-tároló
Ez a példa bemutatja, hogyan Amazon S3 tooan Azure Blob Storage tárolóban toocopy adatait. Azonban adatok átmásolhatók közvetlenül túl[bármely támogatott hello nyelő](data-factory-data-movement-activities.md#supported-data-stores-and-formats) hello másolási tevékenység során a Data Factory használatával.

hello minta biztosít a következő adat-előállító entitások hello JSON jelentésdefiníciókat. A definíciók toocreate egy folyamat toocopy adatok Amazon S3 tooBlob tárolásból hello segítségével is használhatók [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md).   

* A társított szolgáltatás típusa [AwsAccessKey](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [AmazonS3](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) a másolási tevékenység által használt [FileSystemSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello minta másol adatokat az Azure blob Amazon S3 tooan óránként. Ezeket a mintákat használt hello JSON-tulajdonságok hello mintát a következő szakaszok ismertetik.

### <a name="amazon-s3-linked-service"></a>Amazon S3 társított szolgáltatás

```json
{
    "name": "AmazonS3LinkedService",
    "properties": {
        "type": "AwsAccessKey",
        "typeProperties": {
            "accessKeyId": "<access key id>",
            "secretAccessKey": "<secret access key>"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás

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

### <a name="amazon-s3-input-dataset"></a>Amazon S3 bemeneti adatkészlet

Beállítás **"external": true** hello Data Factory szolgáltatásnak arról tájékoztatja, hogy hello dataset külső toohello adat-előállítóban. Ez a tulajdonság tootrue be egy bemeneti adatkészlet nem hello feldolgozási soros tevékenység által létrehozott.

```json
    {
        "name": "AmazonS3InputDataset",
        "properties": {
            "type": "AmazonS3",
            "linkedServiceName": "AmazonS3LinkedService",
            "typeProperties": {
                "key": "testFolder/test.orc",
                "bucketName": "testbucket",
                "format": {
                    "type": "OrcFormat"
                }
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "external": true
        }
    }
```


### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet

Adatot ír tooa új blob minden órában (gyakoriság: óra, időköz: 1). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útját használja hello év, hónap, nap és óra részei hello kezdési ideje.

```json
{
    "name": "AzureBlobOutputDataSet",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/fromamazons3/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
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


### <a name="copy-activity-in-a-pipeline-with-an-amazon-s3-source-and-a-blob-sink"></a>Másolási tevékenység az Amazon S3 és egy blob fogadó egy folyamaton belül

hello folyamat másolatot tevékenységet tartalmaz, amely konfigurált toouse hello bemeneti és kimeneti adatkészletek, és ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello **forrás** típusuk értéke túl**FileSystemSource**, és **fogadó** típusuk értéke túl**BlobSink**.

```json
{
    "name": "CopyAmazonS3ToBlob",
    "properties": {
        "description": "pipeline for copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "FileSystemSource",
                        "recursive": true
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "AmazonS3InputDataset"
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
                "name": "AmazonS3ToBlob"
            }
        ],
        "start": "2014-08-08T18:00:00Z",
        "end": "2014-08-08T19:00:00Z"
    }
}
```
> [!NOTE]
> a forrás adatkészlet toocolumns fogadó adatkészletből toomap oszlopokat lásd [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).


## <a name="next-steps"></a>Következő lépések
Tekintse meg a következő cikkek hello:

* toolearn kulccsal kapcsolatos tényezők adatátvitelt jelölik a (másolási tevékenység) az adat-előállítót, és különböző módokon toooptimize hatás teljesítmény, lásd: hello [másolása tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md).

* A másolási tevékenység során a folyamat létrehozása részletes ismertetését lásd: hello [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
