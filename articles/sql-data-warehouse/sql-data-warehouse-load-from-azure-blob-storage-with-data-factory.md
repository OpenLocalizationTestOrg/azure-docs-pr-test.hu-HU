---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: aaaLoad adatokat az Azure blob storage az Azure SQL Data Warehouse (Azure Data Factory) |} Microsoft Docs
description: "További tudnivalók az Azure Data Factory tooload adatok"
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: 689fb02e-eb98-4f7c-81e6-6c1d22d53901
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: barbkess
ms.custom: loading
ms.openlocfilehash: 29a220679a11cedefb0dfd06c0a6838f81a90447
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-data-from-azure-blob-storage-into-azure-sql-data-warehouse-azure-data-factory"></a><span data-ttu-id="0f3b2-103">Adatok betöltése az Azure Blob Storage-ből az Azure SQL Data Warehouse-ba (Azure Data Factory)</span><span class="sxs-lookup"><span data-stu-id="0f3b2-103">Load data from Azure blob storage into Azure SQL Data Warehouse (Azure Data Factory)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f3b2-104">Data Factory</span><span class="sxs-lookup"><span data-stu-id="0f3b2-104">Data Factory</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-data-factory.md)
> * [<span data-ttu-id="0f3b2-105">PolyBase</span><span class="sxs-lookup"><span data-stu-id="0f3b2-105">PolyBase</span></span>](sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md)
> 
> 

 <span data-ttu-id="0f3b2-106">Az oktatóanyag bemutatja, hogyan toocreate egy folyamatot az Azure Data Factory toomove adatok Azure Storage-Blobba tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-106">This tutorial shows you how toocreate a pipeline in Azure Data Factory toomove data from Azure Storage Blob tooSQL Data Warehouse.</span></span> <span data-ttu-id="0f3b2-107">Az alábbi lépésekkel hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="0f3b2-107">With hello following steps you will:</span></span>

* <span data-ttu-id="0f3b2-108">Mintaadatokat telepíthet egy Azure Storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-108">Set-up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="0f3b2-109">Csatlakoztassa az erőforrások tooAzure adat-Előállítóban.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-109">Connect resources tooAzure Data Factory.</span></span>
* <span data-ttu-id="0f3b2-110">Hozzon létre egy folyamat toomove adatok Storage Blobsba tooSQL Data warehouse-bA.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-110">Create a pipeline toomove data from Storage Blobs tooSQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="0f3b2-111">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="0f3b2-111">Before you begin</span></span>
<span data-ttu-id="0f3b2-112">toofamiliarize saját Azure Data Factory, lásd: [Data Factory bemutatása tooAzure][Introduction tooAzure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-112">toofamiliarize yourself with Azure Data Factory, see [Introduction tooAzure Data Factory][Introduction tooAzure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="0f3b2-113">Erőforrások létrehozása és azonosítása</span><span class="sxs-lookup"><span data-stu-id="0f3b2-113">Create or identify resources</span></span>
<span data-ttu-id="0f3b2-114">Az oktatóanyag elindítása előtt kell a következő erőforrások toohave hello.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-114">Before starting this tutorial, you need toohave hello following resources.</span></span>

* <span data-ttu-id="0f3b2-115">**Azure Storage-Blob**: Ebben az oktatóanyagban használt Azure Storage-Blobba hello adatforrásként hello Azure Data Factory-folyamathoz, és így toohave egy elérhető toostore hello mintaadatokat kell.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-115">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as hello data source for hello Azure Data Factory pipeline, and so you need toohave one available toostore hello sample data.</span></span> <span data-ttu-id="0f3b2-116">Ha Ön nem rendelkezik ilyennel, megtudhatja, hogyan túl[hozzon létre egy tárfiókot][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-116">If you don't have one already, learn how too[Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="0f3b2-117">**Az SQL Data Warehouse**: az Azure Storage-Blobból az oktatóanyag a kurzor hello adatok túl az SQL Data Warehouse, ezért kell egy online adatraktárral, amely be van töltve az AdventureWorksDW-mintaadatok hello toohave.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-117">**SQL Data Warehouse**: This tutorial moves hello data from Azure Storage Blob too SQL Data Warehouse and so need toohave a data warehouse online that is loaded with hello AdventureWorksDW sample data.</span></span> <span data-ttu-id="0f3b2-118">Ha még nem rendelkezik adatraktárral, megtudhatja, hogyan túl[hozhat létre egyet][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-118">If you do not already have a data warehouse, learn how too[provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="0f3b2-119">Ha rendelkezik adatraktárral, de nem töltötte fel hello mintaadatokkal, akkor [töltse be manuálisan][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-119">If you have a data warehouse but didn't provision it with hello sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="0f3b2-120">**Az Azure Data Factory**: Azure Data Factory hello tényleges betöltése befejeződik, és így használható toobuild hello adatok mozgása folyamat egyik toohave kell. Ha Ön nem rendelkezik ilyennel, megtudhatja, hogyan toocreate egy 1. lépésben a [Ismerkedés az Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-120">**Azure Data Factory**: Azure Data Factory will complete hello actual load and so you need toohave one that you can use toobuild hello data movement pipeline.If you don't have one already, learn how toocreate one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="0f3b2-121">**AZCopy**: a helyi ügyfél tooyour Azure Storage-Blobba az AZCopy toocopy hello mintaadatok van szüksége.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-121">**AZCopy**: You need AZCopy toocopy hello sample data from your local client tooyour Azure Storage Blob.</span></span> <span data-ttu-id="0f3b2-122">Telepítési útmutatásért lásd: hello [AZCopy dokumentációját][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-122">For install instructions, see hello [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-tooazure-storage-blob"></a><span data-ttu-id="0f3b2-123">1. lépés: A minta adatok tooAzure tárolási Blob másolása</span><span class="sxs-lookup"><span data-stu-id="0f3b2-123">Step 1: Copy sample data tooAzure Storage Blob</span></span>
<span data-ttu-id="0f3b2-124">Miután az összes hello rendelkezésre, készen áll a toocopy minta adatok tooyour Azure Storage-Blobba áll.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-124">Once you have all of hello pieces ready, you are ready toocopy sample data tooyour Azure Storage Blob.</span></span>

1. <span data-ttu-id="0f3b2-125">[Mintaadatok letöltése][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-125">[Download sample data][Download sample data].</span></span> <span data-ttu-id="0f3b2-126">Ezek az adatok további három év értékesítési adatait tooyour AdventureWorksDW-mintaadatok adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-126">This data will add another three years of sales data tooyour AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="0f3b2-127">Használja az AZCopy parancs toocopy hello három évnyi adat tooyour Azure Storage-Blobba.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-127">Use this AZCopy command toocopy hello three years of data tooyour Azure Storage Blob.</span></span>

````
AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
````


## <a name="step-2-connect-resources-tooazure-data-factory"></a><span data-ttu-id="0f3b2-128">2. lépés: Csatlakozás erőforrások tooAzure adat-előállító</span><span class="sxs-lookup"><span data-stu-id="0f3b2-128">Step 2: Connect resources tooAzure Data Factory</span></span>
<span data-ttu-id="0f3b2-129">Most, hogy rendelkezésre áll hello adatok létrehozhatjuk hello Azure Data Factory adatcsatorna toomove hello adatokat az Azure blob storage az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-129">Now that hello data is in place we can create hello Azure Data Factory pipeline toomove hello data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="0f3b2-130">tooget elindult, nyissa meg hello [Azure-portálon] [ Azure portal] válassza ki a data factory hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-130">tooget started, open hello [Azure portal][Azure portal] and select your data factory from hello left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="0f3b2-131">2.1. lépés: Társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f3b2-131">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="0f3b2-132">Az Azure storage-fiókok és az SQL Data Warehouse tooyour adat-előállító hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-132">Link your Azure storage account and SQL Data Warehouse tooyour data factory.</span></span>  

1. <span data-ttu-id="0f3b2-133">Első lépésként hello regisztrációs folyamat megkezdéséhez kattintson a data factory "Összekapcsolt szolgáltatások" szakaszára hello, és kattintson az "Új adattároló."</span><span class="sxs-lookup"><span data-stu-id="0f3b2-133">First, begin hello registration process by clicking hello 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="0f3b2-134">Válasszon egy nevet tooregister alatt, az típusaként válassza az Azure Storage az azure tárterületet, és írja be a fiók nevét és a Fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-134">Choose a name tooregister your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="0f3b2-135">az SQL Data Warehouse tooregister toohello "Fejlesztés és üzembe helyezés" szakaszban keresse meg, jelölje ki "Új adattároló", majd az "Azure SQL Data Warehouse".</span><span class="sxs-lookup"><span data-stu-id="0f3b2-135">tooregister SQL Data Warehouse navigate toohello 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="0f3b2-136">Másolja és illessze be ezt a sablont, majd adja meg a kért adatokat.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-136">Copy and paste in this template, and then fill in your specific information.</span></span>

```JSON
{
    "name": "<Linked Service Name>",
    "properties": {
        "description": "",
        "type": "AzureSqlDW",
        "typeProperties": {
             "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
         }
    }
}
```

### <a name="step-22-define-hello-dataset"></a><span data-ttu-id="0f3b2-137">2.2. lépés: Hello adatkészlet meghatározása</span><span class="sxs-lookup"><span data-stu-id="0f3b2-137">Step 2.2: Define hello dataset</span></span>
<span data-ttu-id="0f3b2-138">Miután hello létrehozása kapcsolódó szolgáltatások, azt kell toodefine hello adatkészletek.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-138">After creating hello linked services, we will have toodefine hello data sets.</span></span>  <span data-ttu-id="0f3b2-139">Itt Ez azt jelenti, a tárolási tooyour adatraktárból adatbázisba Mozgatott adatok hello hello szerkezete meghatározása.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-139">Here this means defining hello structure of hello data that is being moved from your storage tooyour data warehouse.</span></span>  <span data-ttu-id="0f3b2-140">További információk a létrehozással kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="0f3b2-140">You can read more about creating</span></span>

1. <span data-ttu-id="0f3b2-141">A folyamat indításához lépjen a data factory toohello "Fejlesztés és üzembe helyezés" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-141">Start this process by navigating toohello 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="0f3b2-142">Kattintson "Új adathalmaz" majd "Azure Blob storage" toolink a tárolási tooyour adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-142">Click 'New dataset' and then 'Azure Blob storage' toolink your storage tooyour data factory.</span></span>  <span data-ttu-id="0f3b2-143">Használható parancsfájl toodefine alatt hello az adatokat az Azure Blob storage:</span><span class="sxs-lookup"><span data-stu-id="0f3b2-143">You can use hello below script toodefine your data in Azure Blob storage:</span></span>

```JSON
{
    "name": "<Dataset Name>",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "<linked storage name>",
        "typeProperties": {
            "folderPath": "<containter name>",
            "fileName": "FactInternetSales.csv",
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


1. <span data-ttu-id="0f3b2-144">Most meghatározzuk az adatkészletet az SQL Data Warehouse-hoz.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-144">Now we will also define our dataset for SQL Data Warehouse.</span></span>  <span data-ttu-id="0f3b2-145">Először hello ugyanúgy "Új adathalmaz", majd az "Azure SQL Data Warehouse".</span><span class="sxs-lookup"><span data-stu-id="0f3b2-145">We start in hello same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>

```JSON
{
    "name": "DWDataset",
    "properties": {
        "type": "AzureSqlDWTable",
        "linkedServiceName": "AzureSqlDWLinkedService",
        "typeProperties": {
            "tableName": "FactInternetSales"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="0f3b2-146">3. lépés: A folyamat létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="0f3b2-146">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="0f3b2-147">Végezetül fedi le az Azure Data Factory beállításról és a Futtatás hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-147">Finally, we will set-up and run hello pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="0f3b2-148">Ez az hello művelet végzi el a hello adatok tényleges mozgatását.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-148">This is hello operation that will complete hello actual data movement.</span></span>  <span data-ttu-id="0f3b2-149">Hello műveleteket hajthatja végre az SQL-adatraktár és az Azure Data Factory teljes egészében található [Itt][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-149">You can find a full view of hello operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data tooand from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="0f3b2-150">Hello "Fejlesztés és üzembe helyezés" szakaszban most kattintson a "Több parancs" és "Új adatcsatorna".</span><span class="sxs-lookup"><span data-stu-id="0f3b2-150">In hello 'Author and Deploy' section now click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="0f3b2-151">Hello folyamat létrehozása után a hello kód tootransfer hello adatok tooyour adatraktár alatt is használhatja:</span><span class="sxs-lookup"><span data-stu-id="0f3b2-151">After you create hello pipeline, you can use hello below code tootransfer hello data tooyour data warehouse:</span></span>

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
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
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a><span data-ttu-id="0f3b2-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f3b2-152">Next steps</span></span>
<span data-ttu-id="0f3b2-153">több, először toolearn megtekintése:</span><span class="sxs-lookup"><span data-stu-id="0f3b2-153">toolearn more, start by viewing:</span></span>

* <span data-ttu-id="0f3b2-154">[Azure Data Factory képzési terv][Azure Data Factory learning path].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-154">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="0f3b2-155">[Azure SQL Data Warehouse-összekötő][Azure SQL Data Warehouse Connector].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-155">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="0f3b2-156">Ez a hello fő referencia-témakör az Azure Data Factory használatával az Azure SQL Data Warehouse szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-156">This is hello core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="0f3b2-157">Ezek a témakörök további információkat adnak az Azure Data Factory szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-157">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="0f3b2-158">Azure SQL Database és a HDinsight Témájuk, de hello információk az SQL Data Warehouse tooAzure.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-158">They discuss Azure SQL Database or HDinsight, but hello information also applies tooAzure SQL Data Warehouse.</span></span>

* <span data-ttu-id="0f3b2-159">[Oktatóanyag: Ismerkedés az Azure Data Factoryvel] [ Tutorial: Get started with Azure Data Factory] hello fő oktatóanyag az Azure Data Factory adatfeldolgozás azt.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-159">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is hello core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="0f3b2-160">Ebben az oktatóanyagban felépítheti első folyamatát, amely a HDInsight tootransform használja, és havonta webes naplók elemzése.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-160">In this tutorial you will build your first pipeline that uses HDInsight tootransform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="0f3b2-161">Megjegyzés: Ebben az oktatóanyagban nincs másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-161">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="0f3b2-162">[Oktatóanyag: Adatok másolása az Azure Storage-Blobba tooAzure SQL-adatbázis][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="0f3b2-162">[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database][Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database].</span></span> <span data-ttu-id="0f3b2-163">Ebben az oktatóanyagban az Azure Storage-Blobba tooAzure SQL-adatbázis egy folyamatot az Azure Data Factory toocopy adatokat hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0f3b2-163">In this tutorial, you will create a pipeline in Azure Data Factory toocopy data from Azure Storage Blob tooAzure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction tooAzure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data tooand from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob tooAzure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
