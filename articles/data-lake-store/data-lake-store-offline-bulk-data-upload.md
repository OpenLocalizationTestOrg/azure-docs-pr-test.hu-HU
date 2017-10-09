---
title: "kapcsolat nélküli módszerrel adatok Data Lake Store nagy mennyiségű aaaUpload |} Microsoft Docs"
description: "Használjon hello AdlCopy eszköz toocopy adatokat az Azure Storage-blobok tooData Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 45321f6a-179f-4ee4-b8aa-efa7745b8eb6
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: 42ef75142a26ebfab05d89614782a54c244c4bcb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a>Hello Azure Import/Export szolgáltatás tooData Lake adattárban offline másolatának használata
Ebből a cikkből megtudhatja, hogyan toocopy túl nagy adatkészletek (> 200 GB-os) az Azure Data Lake Store offline másolat módszerek, például a hello segítségével történő [Azure Import/Export szolgáltatás](../storage/common/storage-import-export-service.md). Ebben a cikkben példaként használt hello fájl többek között 339,420,860,416 bájt vagy körülbelül 319 GB-TAL a lemezen. Most hívja meg a fájl 319GB.tsv.

hello Azure Import/Export szolgáltatás segít tootransfer nagymennyiségű adat további biztonságosan tooAzure Blob-tároló szállítási merevlemez-meghajtók tooan Azure-adatközpontban.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt hozzákezd, rendelkeznie kell hello következő:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).
* **Azure-tárfiók**.
* **Egy Azure Data Lake Store-fiók**. Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)

## <a name="preparing-hello-data"></a>Hello adatok előkészítése
Hello Import/Export szolgáltatás használata előtt break hello adatok fájl toobe átvitt **azokat, amelyek 200 GB-nál kisebb másolatok** mérete. hello import eszközt 200 GB-nál nagyobb fájlokat nem működik. Az oktatóanyag azt felosztása hello fájl adattömbök 100 GB. Ehhez a [Cygwin](https://cygwin.com/install.html). Cygwin támogatja a Linux-parancsok. Ebben az esetben használja a következő parancs hello:

    split -b 100m 319GB.tsv

hello split művelet a következő neveket hello hozza létre a fájlokat.

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a>Felkészülés a lemezek adatokkal
Hello utasításait követve [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) (alatt hello **készítse elő a meghajtók** szakaszban) tooprepare a merevlemez-meghajtók. Itt az általános feladatütemezési hello:

1. Be kell szereznie egy merevlemezt, amely megfelel a hello követelmény toobe hello Azure Import/Export szolgáltatás használt.
2. Hello adatok másolatának tárolására után az Azure-adatközpontban szállított toohello Azure storage-fiók azonosítása.
3. Használjon hello [Azure Import/Export eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), parancssori segédprogramot. Íme egy minta kódrészletet, amely bemutatja, hogyan toouse hello eszköz.

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    Lásd: [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) a minta további részletek.
4. hello előző parancs naplót hoz létre egy fájlban a következő hello megadott helyre. A napló fájl toocreate az importálási feladat a hello használata [a klasszikus Azure portálon](https://manage.windowsazure.com).

## <a name="create-an-import-job"></a>Importálási feladat létrehozása
Mostantól létrehozhat egy importálási feladat hello utasításait használatával [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) (hello alatt **létrehozás hello importálási feladat** szakaszban). Az importálási feladat más adatokkal is adja meg hello lemezmeghajtók előkészítése során létrehozott hello napló fájlt.

## <a name="physically-ship-hello-disks"></a>Fizikailag küldje el a hello lemezek
Fizikailag most szállított hello lemezek tooan Azure-adatközpontban. Itt hello adatok másolásakor toohello Azure Storage blobs hello importálási feladat létrehozásakor megadott keresztül. Is hello feladat létrehozásakor választotta tooprovide hello nyomkövetési adatokat később, ha most lépjen vissza tooyour importálási feladat és a frissítés hello követési szám.

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a>Adatok másolása az Azure Storage blobs tooAzure Data Lake Store
Hello hello állapotának után importálási feladat, hogy végzett, ellenőrizheti, hogy hello Azure Storage blobs volt megadva az rendelkezésre áll-e hello adatokat jeleníti meg. Módszerek toomove, hogy hello adatait blobok tooAzure Data Lake Store számos használhatja. Elérhető lehetőségek adatfeltöltési hello összes, a következő témakörben: [adatok bevitele a Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).

Ez a szakasz azt biztosít hello JSON-definíciók használható toocreate egy Azure Data Factory-folyamat az adatok másolása. Használhatja a JSON-definíciókban hello [Azure-portálon](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), vagy [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).

### <a name="source-linked-service-azure-storage-blob"></a>Forrás társított szolgáltatás (Azure Storage-blobba)
````
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "description": "",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
````

### <a name="target-linked-service-azure-data-lake-store"></a>Cél társított szolgáltatás (az Azure Data Lake Store)
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' tooallow this data factory and hello activities it runs tooaccess this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from hello OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a>Bemeneti adatkészlet
````
{
    "name": "InputDataSet",
    "properties": {
        "published": false,
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
        "typeProperties": {
            "folderPath": "importcontainer/vf1/"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {}
    }
}
````
### <a name="output-data-set"></a>Kimeneti adatkészlet
````
{
"name": "OutputDataSet",
"properties": {
  "published": false,
  "type": "AzureDataLakeStore",
  "linkedServiceName": "AzureDataLakeStoreLinkedService",
  "typeProperties": {
    "folderPath": "/importeddatafeb8job/"
    },
  "availability": {
    "frequency": "Hour",
    "interval": 1
    }
  }
}
````
### <a name="pipeline-copy-activity"></a>Feldolgozási sor (másolási tevékenység)
````
{
    "name": "CopyImportedData",
    "properties": {
        "description": "Pipeline with copy activity",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "AzureDataLakeStoreSink",
                        "copyBehavior": "PreserveHierarchy",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [
                    {
                        "name": "InputDataSet"
                    }
                ],
                "outputs": [
                    {
                        "name": "OutputDataSet"
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
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity"
            }
        ],
        "start": "2016-02-08T22:00:00Z",
        "end": "2016-02-08T23:00:00Z",
        "isPaused": false,
        "pipelineMode": "Scheduled"
    }
}
````
További információkért lásd: [áthelyezni az adatokat az Azure Storage blob tooAzure Data Lake Store használatának Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a>Hozza létre újból az Azure Data Lake Store-adatfájlok hello
A fájl, 319 GB, és elromlik azt kisebb méretű fájlok be, hogy hello Azure Import/Export szolgáltatás segítségével sikerült továbbítani használatába azt. Most, hogy hello adatokat az Azure Data Lake Store, azt is helyreállítására hello tooits eredeti méretét. Ezért a következő Azure PowerShell cmldts toodo hello is használhatja.

````
# Login tooour account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch toohello subscription you want toowork with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  hello files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a>Következő lépések
* [Biztonságos adattárolás a Data Lake Store-ban](data-lake-store-secure-data.md)
* [Az Azure Data Lake Analytics használata a Data Lake Store-ral](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Az Azure HDInsight használata a Data Lake Store-ral](data-lake-store-hdinsight-hadoop-use-portal.md)
