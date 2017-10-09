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
# <a name="use-hello-azure-importexport-service-for-offline-copy-of-data-toodata-lake-store"></a><span data-ttu-id="09beb-103">Hello Azure Import/Export szolgáltatás tooData Lake adattárban offline másolatának használata</span><span class="sxs-lookup"><span data-stu-id="09beb-103">Use hello Azure Import/Export service for offline copy of data tooData Lake Store</span></span>
<span data-ttu-id="09beb-104">Ebből a cikkből megtudhatja, hogyan toocopy túl nagy adatkészletek (> 200 GB-os) az Azure Data Lake Store offline másolat módszerek, például a hello segítségével történő [Azure Import/Export szolgáltatás](../storage/common/storage-import-export-service.md).</span><span class="sxs-lookup"><span data-stu-id="09beb-104">In this article, you'll learn how toocopy huge data sets (>200 GB) into an Azure Data Lake Store by using offline copy methods, like hello [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span></span> <span data-ttu-id="09beb-105">Ebben a cikkben példaként használt hello fájl többek között 339,420,860,416 bájt vagy körülbelül 319 GB-TAL a lemezen.</span><span class="sxs-lookup"><span data-stu-id="09beb-105">Specifically, hello file used as an example in this article is 339,420,860,416 bytes, or about 319 GB on disk.</span></span> <span data-ttu-id="09beb-106">Most hívja meg a fájl 319GB.tsv.</span><span class="sxs-lookup"><span data-stu-id="09beb-106">Let's call this file 319GB.tsv.</span></span>

<span data-ttu-id="09beb-107">hello Azure Import/Export szolgáltatás segít tootransfer nagymennyiségű adat további biztonságosan tooAzure Blob-tároló szállítási merevlemez-meghajtók tooan Azure-adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="09beb-107">hello Azure Import/Export service helps you tootransfer large amounts of data more securely tooAzure Blob storage by shipping hard disk drives tooan Azure datacenter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="09beb-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="09beb-108">Prerequisites</span></span>
<span data-ttu-id="09beb-109">Mielőtt hozzákezd, rendelkeznie kell hello következő:</span><span class="sxs-lookup"><span data-stu-id="09beb-109">Before you begin, you must have hello following:</span></span>

* <span data-ttu-id="09beb-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="09beb-110">**An Azure subscription**.</span></span> <span data-ttu-id="09beb-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="09beb-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="09beb-112">**Azure-tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="09beb-112">**An Azure storage account**.</span></span>
* <span data-ttu-id="09beb-113">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="09beb-113">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="09beb-114">Útmutatást toocreate egy, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="09beb-114">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

## <a name="preparing-hello-data"></a><span data-ttu-id="09beb-115">Hello adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="09beb-115">Preparing hello data</span></span>
<span data-ttu-id="09beb-116">Hello Import/Export szolgáltatás használata előtt break hello adatok fájl toobe átvitt **azokat, amelyek 200 GB-nál kisebb másolatok** mérete.</span><span class="sxs-lookup"><span data-stu-id="09beb-116">Before using hello Import/Export service, break hello data file toobe transferred **into copies that are less than 200 GB** in size.</span></span> <span data-ttu-id="09beb-117">hello import eszközt 200 GB-nál nagyobb fájlokat nem működik.</span><span class="sxs-lookup"><span data-stu-id="09beb-117">hello import tool does not work with files greater than 200 GB.</span></span> <span data-ttu-id="09beb-118">Az oktatóanyag azt felosztása hello fájl adattömbök 100 GB.</span><span class="sxs-lookup"><span data-stu-id="09beb-118">In this tutorial, we split hello file into chunks of 100 GB each.</span></span> <span data-ttu-id="09beb-119">Ehhez a [Cygwin](https://cygwin.com/install.html).</span><span class="sxs-lookup"><span data-stu-id="09beb-119">You can do this by using [Cygwin](https://cygwin.com/install.html).</span></span> <span data-ttu-id="09beb-120">Cygwin támogatja a Linux-parancsok.</span><span class="sxs-lookup"><span data-stu-id="09beb-120">Cygwin supports Linux commands.</span></span> <span data-ttu-id="09beb-121">Ebben az esetben használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="09beb-121">In this case, use hello following command:</span></span>

    split -b 100m 319GB.tsv

<span data-ttu-id="09beb-122">hello split művelet a következő neveket hello hozza létre a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="09beb-122">hello split operation creates files with hello following names.</span></span>

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a><span data-ttu-id="09beb-123">Felkészülés a lemezek adatokkal</span><span class="sxs-lookup"><span data-stu-id="09beb-123">Get disks ready with data</span></span>
<span data-ttu-id="09beb-124">Hello utasításait követve [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) (alatt hello **készítse elő a meghajtók** szakaszban) tooprepare a merevlemez-meghajtók.</span><span class="sxs-lookup"><span data-stu-id="09beb-124">Follow hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Prepare your drives** section) tooprepare your hard drives.</span></span> <span data-ttu-id="09beb-125">Itt az általános feladatütemezési hello:</span><span class="sxs-lookup"><span data-stu-id="09beb-125">Here's hello overall sequence:</span></span>

1. <span data-ttu-id="09beb-126">Be kell szereznie egy merevlemezt, amely megfelel a hello követelmény toobe hello Azure Import/Export szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="09beb-126">Procure a hard disk that meets hello requirement toobe used for hello Azure Import/Export service.</span></span>
2. <span data-ttu-id="09beb-127">Hello adatok másolatának tárolására után az Azure-adatközpontban szállított toohello Azure storage-fiók azonosítása.</span><span class="sxs-lookup"><span data-stu-id="09beb-127">Identify an Azure storage account where hello data will be copied after it is shipped toohello Azure datacenter.</span></span>
3. <span data-ttu-id="09beb-128">Használjon hello [Azure Import/Export eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), parancssori segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="09beb-128">Use hello [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), a command-line utility.</span></span> <span data-ttu-id="09beb-129">Íme egy minta kódrészletet, amely bemutatja, hogyan toouse hello eszköz.</span><span class="sxs-lookup"><span data-stu-id="09beb-129">Here's a sample snippet that shows how toouse hello tool.</span></span>

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    <span data-ttu-id="09beb-130">Lásd: [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) a minta további részletek.</span><span class="sxs-lookup"><span data-stu-id="09beb-130">See [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) for more sample snippets.</span></span>
4. <span data-ttu-id="09beb-131">hello előző parancs naplót hoz létre egy fájlban a következő hello megadott helyre.</span><span class="sxs-lookup"><span data-stu-id="09beb-131">hello preceding command creates a journal file at hello specified location.</span></span> <span data-ttu-id="09beb-132">A napló fájl toocreate az importálási feladat a hello használata [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="09beb-132">Use this journal file toocreate an import job from hello [Azure classic portal](https://manage.windowsazure.com).</span></span>

## <a name="create-an-import-job"></a><span data-ttu-id="09beb-133">Importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="09beb-133">Create an import job</span></span>
<span data-ttu-id="09beb-134">Mostantól létrehozhat egy importálási feladat hello utasításait használatával [hello Azure Import/Export szolgáltatás használata](../storage/common/storage-import-export-service.md) (hello alatt **létrehozás hello importálási feladat** szakaszban).</span><span class="sxs-lookup"><span data-stu-id="09beb-134">You can now create an import job by using hello instructions in [Using hello Azure Import/Export service](../storage/common/storage-import-export-service.md) (under hello **Create hello Import job** section).</span></span> <span data-ttu-id="09beb-135">Az importálási feladat más adatokkal is adja meg hello lemezmeghajtók előkészítése során létrehozott hello napló fájlt.</span><span class="sxs-lookup"><span data-stu-id="09beb-135">For this import job, with other details, also provide hello journal file created while preparing hello disk drives.</span></span>

## <a name="physically-ship-hello-disks"></a><span data-ttu-id="09beb-136">Fizikailag küldje el a hello lemezek</span><span class="sxs-lookup"><span data-stu-id="09beb-136">Physically ship hello disks</span></span>
<span data-ttu-id="09beb-137">Fizikailag most szállított hello lemezek tooan Azure-adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="09beb-137">You can now physically ship hello disks tooan Azure datacenter.</span></span> <span data-ttu-id="09beb-138">Itt hello adatok másolásakor toohello Azure Storage blobs hello importálási feladat létrehozásakor megadott keresztül.</span><span class="sxs-lookup"><span data-stu-id="09beb-138">There, hello data is copied over toohello Azure Storage blobs you provided while creating hello import job.</span></span> <span data-ttu-id="09beb-139">Is hello feladat létrehozásakor választotta tooprovide hello nyomkövetési adatokat később, ha most lépjen vissza tooyour importálási feladat és a frissítés hello követési szám.</span><span class="sxs-lookup"><span data-stu-id="09beb-139">Also, while creating hello job, if you opted tooprovide hello tracking information later, you can now go back tooyour import job and update hello tracking number.</span></span>

## <a name="copy-data-from-azure-storage-blobs-tooazure-data-lake-store"></a><span data-ttu-id="09beb-140">Adatok másolása az Azure Storage blobs tooAzure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="09beb-140">Copy data from Azure Storage blobs tooAzure Data Lake Store</span></span>
<span data-ttu-id="09beb-141">Hello hello állapotának után importálási feladat, hogy végzett, ellenőrizheti, hogy hello Azure Storage blobs volt megadva az rendelkezésre áll-e hello adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="09beb-141">After hello status of hello import job shows that it's completed, you can verify whether hello data is available in hello Azure Storage blobs you had specified.</span></span> <span data-ttu-id="09beb-142">Módszerek toomove, hogy hello adatait blobok tooAzure Data Lake Store számos használhatja.</span><span class="sxs-lookup"><span data-stu-id="09beb-142">You can then use a variety of methods toomove that data from hello blobs tooAzure Data Lake Store.</span></span> <span data-ttu-id="09beb-143">Elérhető lehetőségek adatfeltöltési hello összes, a következő témakörben: [adatok bevitele a Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="09beb-143">For all hello available options for uploading data, see [Ingesting data into Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span></span>

<span data-ttu-id="09beb-144">Ez a szakasz azt biztosít hello JSON-definíciók használható toocreate egy Azure Data Factory-folyamat az adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="09beb-144">In this section, we provide you with hello JSON definitions that you can use toocreate an Azure Data Factory pipeline for copying data.</span></span> <span data-ttu-id="09beb-145">Használhatja a JSON-definíciókban hello [Azure-portálon](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), vagy [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="09beb-145">You can use these JSON definitions from hello [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), or [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

### <a name="source-linked-service-azure-storage-blob"></a><span data-ttu-id="09beb-146">Forrás társított szolgáltatás (Azure Storage-blobba)</span><span class="sxs-lookup"><span data-stu-id="09beb-146">Source linked service (Azure Storage blob)</span></span>
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

### <a name="target-linked-service-azure-data-lake-store"></a><span data-ttu-id="09beb-147">Cél társított szolgáltatás (az Azure Data Lake Store)</span><span class="sxs-lookup"><span data-stu-id="09beb-147">Target linked service (Azure Data Lake Store)</span></span>
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
### <a name="input-data-set"></a><span data-ttu-id="09beb-148">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="09beb-148">Input data set</span></span>
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
### <a name="output-data-set"></a><span data-ttu-id="09beb-149">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="09beb-149">Output data set</span></span>
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
### <a name="pipeline-copy-activity"></a><span data-ttu-id="09beb-150">Feldolgozási sor (másolási tevékenység)</span><span class="sxs-lookup"><span data-stu-id="09beb-150">Pipeline (copy activity)</span></span>
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
<span data-ttu-id="09beb-151">További információkért lásd: [áthelyezni az adatokat az Azure Storage blob tooAzure Data Lake Store használatának Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span><span class="sxs-lookup"><span data-stu-id="09beb-151">For more information, see [Move data from Azure Storage blob tooAzure Data Lake Store using Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span></span>

## <a name="reconstruct-hello-data-files-in-azure-data-lake-store"></a><span data-ttu-id="09beb-152">Hozza létre újból az Azure Data Lake Store-adatfájlok hello</span><span class="sxs-lookup"><span data-stu-id="09beb-152">Reconstruct hello data files in Azure Data Lake Store</span></span>
<span data-ttu-id="09beb-153">A fájl, 319 GB, és elromlik azt kisebb méretű fájlok be, hogy hello Azure Import/Export szolgáltatás segítségével sikerült továbbítani használatába azt.</span><span class="sxs-lookup"><span data-stu-id="09beb-153">We started with a file that was 319 GB, and broke it down into files of smaller size so that it could be transferred by using hello Azure Import/Export service.</span></span> <span data-ttu-id="09beb-154">Most, hogy hello adatokat az Azure Data Lake Store, azt is helyreállítására hello tooits eredeti méretét.</span><span class="sxs-lookup"><span data-stu-id="09beb-154">Now that hello data is in Azure Data Lake Store, we can reconstruct hello file tooits original size.</span></span> <span data-ttu-id="09beb-155">Ezért a következő Azure PowerShell cmldts toodo hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="09beb-155">You can use hello following Azure PowerShell cmldts toodo so.</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="09beb-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="09beb-156">Next steps</span></span>
* [<span data-ttu-id="09beb-157">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="09beb-157">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="09beb-158">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="09beb-158">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="09beb-159">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="09beb-159">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
