---
title: "Nagy mennyiségű adatok feltöltése a Data Lake Store kapcsolat nélküli módszerrel |} Microsoft Docs"
description: "Adatok másolása az Azure Storage blobs Data Lake store a AdlCopy eszközzel"
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
ms.openlocfilehash: b469c0ebe9838a1ea986cff3043e3008941e9aa9
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-the-azure-importexport-service-for-offline-copy-of-data-to-data-lake-store"></a><span data-ttu-id="4ffc9-103">Data Lake Store-adatok offline példányát az Azure Import/Export szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="4ffc9-103">Use the Azure Import/Export service for offline copy of data to Data Lake Store</span></span>
<span data-ttu-id="4ffc9-104">Ebből a cikkből megtudhatja, hogyan hatalmas adatok másolása (> 200 GB-os) azokat az Azure Data Lake Store módszerrel offline másolat, például a [Azure Import/Export szolgáltatás](../storage/common/storage-import-export-service.md).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-104">In this article, you'll learn how to copy huge data sets (>200 GB) into an Azure Data Lake Store by using offline copy methods, like the [Azure Import/Export service](../storage/common/storage-import-export-service.md).</span></span> <span data-ttu-id="4ffc9-105">Pontosabban ebben a cikkben példa fájl 339,420,860,416 bájt vagy körülbelül 319 GB-TAL a lemezen.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-105">Specifically, the file used as an example in this article is 339,420,860,416 bytes, or about 319 GB on disk.</span></span> <span data-ttu-id="4ffc9-106">Most hívja meg a fájl 319GB.tsv.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-106">Let's call this file 319GB.tsv.</span></span>

<span data-ttu-id="4ffc9-107">Az Azure Import/Export szolgáltatás segítségével átvitele nagy mennyiségű adatok biztonsága érdekében az Azure Blob storage szállítási merevlemez-meghajtók számára egy Azure-adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-107">The Azure Import/Export service helps you to transfer large amounts of data more securely to Azure Blob storage by shipping hard disk drives to an Azure datacenter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4ffc9-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4ffc9-108">Prerequisites</span></span>
<span data-ttu-id="4ffc9-109">Mielőtt hozzákezd, rendelkeznie kell a következő:</span><span class="sxs-lookup"><span data-stu-id="4ffc9-109">Before you begin, you must have the following:</span></span>

* <span data-ttu-id="4ffc9-110">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-110">**An Azure subscription**.</span></span> <span data-ttu-id="4ffc9-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="4ffc9-112">**Azure-tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-112">**An Azure storage account**.</span></span>
* <span data-ttu-id="4ffc9-113">**Egy Azure Data Lake Store-fiók**.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-113">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="4ffc9-114">Hogyan hozhat létre ilyet, lásd: [Ismerkedés az Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="4ffc9-114">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>

## <a name="preparing-the-data"></a><span data-ttu-id="4ffc9-115">Az adatok előkészítése</span><span class="sxs-lookup"><span data-stu-id="4ffc9-115">Preparing the data</span></span>
<span data-ttu-id="4ffc9-116">A Import/Export szolgáltatás használata előtt át kell helyezni az adatfájl törés **azokat, amelyek 200 GB-nál kisebb másolatok** mérete.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-116">Before using the Import/Export service, break the data file to be transferred **into copies that are less than 200 GB** in size.</span></span> <span data-ttu-id="4ffc9-117">200 GB-nál nagyobb fájlokat nem működik az import eszközt.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-117">The import tool does not work with files greater than 200 GB.</span></span> <span data-ttu-id="4ffc9-118">Az oktatóanyag azt ossza fel a fájl minden 100 GB méretű adattömböket írnak.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-118">In this tutorial, we split the file into chunks of 100 GB each.</span></span> <span data-ttu-id="4ffc9-119">Ehhez a [Cygwin](https://cygwin.com/install.html).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-119">You can do this by using [Cygwin](https://cygwin.com/install.html).</span></span> <span data-ttu-id="4ffc9-120">Cygwin támogatja a Linux-parancsok.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-120">Cygwin supports Linux commands.</span></span> <span data-ttu-id="4ffc9-121">Ebben az esetben használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="4ffc9-121">In this case, use the following command:</span></span>

    split -b 100m 319GB.tsv

<span data-ttu-id="4ffc9-122">A felosztott művelet a következő nevű fájlokat hozza létre.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-122">The split operation creates files with the following names.</span></span>

    319GB.tsv-part-aa

    319GB.tsv-part-ab

    319GB.tsv-part-ac

    319GB.tsv-part-ad

## <a name="get-disks-ready-with-data"></a><span data-ttu-id="4ffc9-123">Felkészülés a lemezek adatokkal</span><span class="sxs-lookup"><span data-stu-id="4ffc9-123">Get disks ready with data</span></span>
<span data-ttu-id="4ffc9-124">Kövesse az utasításokat a [az Azure Import/Export szolgáltatás használatával](../storage/common/storage-import-export-service.md) (alatt a **készítse elő a meghajtók** szakasz) a merevlemez-meghajtók előkészítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-124">Follow the instructions in [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) (under the **Prepare your drives** section) to prepare your hard drives.</span></span> <span data-ttu-id="4ffc9-125">Az általános feladatütemezési itt található:</span><span class="sxs-lookup"><span data-stu-id="4ffc9-125">Here's the overall sequence:</span></span>

1. <span data-ttu-id="4ffc9-126">Be kell szereznie egy merevlemezt, amely megfelel a követelmény az Azure Import/Export szolgáltatás használt.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-126">Procure a hard disk that meets the requirement to be used for the Azure Import/Export service.</span></span>
2. <span data-ttu-id="4ffc9-127">Az adatok másolatának tárolására után a szállított az Azure-adatközpontban az Azure storage-fiók azonosítása.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-127">Identify an Azure storage account where the data will be copied after it is shipped to the Azure datacenter.</span></span>
3. <span data-ttu-id="4ffc9-128">Használja a [Azure Import/Export eszköz](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), parancssori segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-128">Use the [Azure Import/Export Tool](http://go.microsoft.com/fwlink/?LinkID=301900&clcid=0x409), a command-line utility.</span></span> <span data-ttu-id="4ffc9-129">Íme egy minta kódrészletet, amely bemutatja, hogyan használhatja az eszközt.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-129">Here's a sample snippet that shows how to use the tool.</span></span>

    ````
    WAImportExport PrepImport /sk:<StorageAccountKey> /t: <TargetDriveLetter> /format /encrypt /logdir:e:\myexportimportjob\logdir /j:e:\myexportimportjob\journal1.jrn /id:myexportimportjob /srcdir:F:\demo\ExImContainer /dstdir:importcontainer/vf1/
    ````
    <span data-ttu-id="4ffc9-130">Lásd: [az Azure Import/Export szolgáltatás használatával](../storage/common/storage-import-export-service.md) a minta további részletek.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-130">See [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) for more sample snippets.</span></span>
4. <span data-ttu-id="4ffc9-131">Az előző parancs létrehoz egy napló fájlt a megadott helyen.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-131">The preceding command creates a journal file at the specified location.</span></span> <span data-ttu-id="4ffc9-132">Ez a napló fájl segítségével az importálási feladat létrehozása a [a klasszikus Azure portálon](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-132">Use this journal file to create an import job from the [Azure classic portal](https://manage.windowsazure.com).</span></span>

## <a name="create-an-import-job"></a><span data-ttu-id="4ffc9-133">Importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="4ffc9-133">Create an import job</span></span>
<span data-ttu-id="4ffc9-134">Mostantól létrehozhat egy importálási feladat található utasítások segítségével [az Azure Import/Export szolgáltatás használatával](../storage/common/storage-import-export-service.md) (alatt a **az importálási feladat létrehozása** szakaszban).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-134">You can now create an import job by using the instructions in [Using the Azure Import/Export service](../storage/common/storage-import-export-service.md) (under the **Create the Import job** section).</span></span> <span data-ttu-id="4ffc9-135">Az importálási feladathoz más adatokkal is adja meg a napló-fájlját a merevlemez-meghajtók előkészítése során létrehozott.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-135">For this import job, with other details, also provide the journal file created while preparing the disk drives.</span></span>

## <a name="physically-ship-the-disks"></a><span data-ttu-id="4ffc9-136">Fizikailag küldje el a lemezek</span><span class="sxs-lookup"><span data-stu-id="4ffc9-136">Physically ship the disks</span></span>
<span data-ttu-id="4ffc9-137">Fizikailag most szállítása a lemezt egy Azure-adatközpontban.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-137">You can now physically ship the disks to an Azure datacenter.</span></span> <span data-ttu-id="4ffc9-138">Hiba, a adatokat másolja át az Azure Storage blobs szolgáltatásban, az importálási feladat létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-138">There, the data is copied over to the Azure Storage blobs you provided while creating the import job.</span></span> <span data-ttu-id="4ffc9-139">Is a feladat létrehozásakor, ha később, a nyomon követési információk választotta mostantól térjen vissza az importálás és frissítés a nyomon követési száma.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-139">Also, while creating the job, if you opted to provide the tracking information later, you can now go back to your import job and update the tracking number.</span></span>

## <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-store"></a><span data-ttu-id="4ffc9-140">Adatok másolása az Azure Storage blobs szolgáltatásban az Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="4ffc9-140">Copy data from Azure Storage blobs to Azure Data Lake Store</span></span>
<span data-ttu-id="4ffc9-141">Után az importálási feladat állapotát jeleníti meg, hogy végzett, ellenőrizheti, hogy az adatok érhető el az Azure Storage blobs volt megadva.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-141">After the status of the import job shows that it's completed, you can verify whether the data is available in the Azure Storage blobs you had specified.</span></span> <span data-ttu-id="4ffc9-142">Számos módszer a blobok adatok áthelyezése az Azure Data Lake Store használhatja.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-142">You can then use a variety of methods to move that data from the blobs to Azure Data Lake Store.</span></span> <span data-ttu-id="4ffc9-143">Az összes elérhető beállítások adatfeltöltési, lásd: [adatok bevitele a Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-143">For all the available options for uploading data, see [Ingesting data into Data Lake Store](data-lake-store-data-scenarios.md#ingest-data-into-data-lake-store).</span></span>

<span data-ttu-id="4ffc9-144">Ez a szakasz azt adja meg a JSON-definíciók, amelyek segítségével hozzon létre egy Azure Data Factory-folyamat az adatok másolása.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-144">In this section, we provide you with the JSON definitions that you can use to create an Azure Data Factory pipeline for copying data.</span></span> <span data-ttu-id="4ffc9-145">A JSON-definíciókban is használhatja a [Azure-portálon](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), vagy [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-145">You can use these JSON definitions from the [Azure portal](../data-factory/data-factory-copy-activity-tutorial-using-azure-portal.md), or [Visual Studio](../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md), or [Azure PowerShell](../data-factory/data-factory-copy-activity-tutorial-using-powershell.md).</span></span>

### <a name="source-linked-service-azure-storage-blob"></a><span data-ttu-id="4ffc9-146">Forrás társított szolgáltatás (Azure Storage-blobba)</span><span class="sxs-lookup"><span data-stu-id="4ffc9-146">Source linked service (Azure Storage blob)</span></span>
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

### <a name="target-linked-service-azure-data-lake-store"></a><span data-ttu-id="4ffc9-147">Cél társított szolgáltatás (az Azure Data Lake Store)</span><span class="sxs-lookup"><span data-stu-id="4ffc9-147">Target linked service (Azure Data Lake Store)</span></span>
````
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "description": "",
        "typeProperties": {
            "authorization": "<Click 'Authorize' to allow this data factory and the activities it runs to access this Data Lake Store with your access rights>",
            "dataLakeStoreUri": "https://<adls_account_name>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<OAuth session id from the OAuth authorization session. Each session id is unique and may only be used once>"
        }
    }
}
````
### <a name="input-data-set"></a><span data-ttu-id="4ffc9-148">Bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="4ffc9-148">Input data set</span></span>
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
### <a name="output-data-set"></a><span data-ttu-id="4ffc9-149">Kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="4ffc9-149">Output data set</span></span>
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
### <a name="pipeline-copy-activity"></a><span data-ttu-id="4ffc9-150">Feldolgozási sor (másolási tevékenység)</span><span class="sxs-lookup"><span data-stu-id="4ffc9-150">Pipeline (copy activity)</span></span>
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
<span data-ttu-id="4ffc9-151">További információkért lásd: [helyezze át az adatokat az Azure Storage-blobból az Azure Data Lake Store Azure Data Factory használatával](../data-factory/data-factory-azure-datalake-connector.md).</span><span class="sxs-lookup"><span data-stu-id="4ffc9-151">For more information, see [Move data from Azure Storage blob to Azure Data Lake Store using Azure Data Factory](../data-factory/data-factory-azure-datalake-connector.md).</span></span>

## <a name="reconstruct-the-data-files-in-azure-data-lake-store"></a><span data-ttu-id="4ffc9-152">Hozza létre újból az Azure Data Lake Store-adatfájlok</span><span class="sxs-lookup"><span data-stu-id="4ffc9-152">Reconstruct the data files in Azure Data Lake Store</span></span>
<span data-ttu-id="4ffc9-153">A fájl, 319 GB, és úgy, hogy az Azure Import/Export szolgáltatás használatával sikerült továbbítani túllépte azt le a kisebb méretű fájlok használatába azt.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-153">We started with a file that was 319 GB, and broke it down into files of smaller size so that it could be transferred by using the Azure Import/Export service.</span></span> <span data-ttu-id="4ffc9-154">Most, hogy az adatok Azure Data Lake Store-ban, hogy a fájl eredeti méretének is helyreállítására.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-154">Now that the data is in Azure Data Lake Store, we can reconstruct the file to its original size.</span></span> <span data-ttu-id="4ffc9-155">A következő Azure PowerShell cmldts ehhez használhatja.</span><span class="sxs-lookup"><span data-stu-id="4ffc9-155">You can use the following Azure PowerShell cmldts to do so.</span></span>

````
# Login to our account
Login-AzureRmAccount

# List your subscriptions
Get-AzureRmSubscription

# Switch to the subscription you want to work with
Set-AzureRmContext –SubscriptionId
Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"

# Join  the files
Join-AzureRmDataLakeStoreItem -AccountName "<adls_account_name" -Paths "/importeddatafeb8job/319GB.tsv-part-aa","/importeddatafeb8job/319GB.tsv-part-ab", "/importeddatafeb8job/319GB.tsv-part-ac", "/importeddatafeb8job/319GB.tsv-part-ad" -Destination "/importeddatafeb8job/MergedFile.csv”
````

## <a name="next-steps"></a><span data-ttu-id="4ffc9-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ffc9-156">Next steps</span></span>
* [<span data-ttu-id="4ffc9-157">Biztonságos adattárolás a Data Lake Store-ban</span><span class="sxs-lookup"><span data-stu-id="4ffc9-157">Secure data in Data Lake Store</span></span>](data-lake-store-secure-data.md)
* [<span data-ttu-id="4ffc9-158">Az Azure Data Lake Analytics használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="4ffc9-158">Use Azure Data Lake Analytics with Data Lake Store</span></span>](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [<span data-ttu-id="4ffc9-159">Az Azure HDInsight használata a Data Lake Store-ral</span><span class="sxs-lookup"><span data-stu-id="4ffc9-159">Use Azure HDInsight with Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
