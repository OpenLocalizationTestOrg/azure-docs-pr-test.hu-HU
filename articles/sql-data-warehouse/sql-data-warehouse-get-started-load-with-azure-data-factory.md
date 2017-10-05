---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "Adatok betöltése az Azure Data Factoryvel | Microsoft Docs"
description: "Sajátítsa el az adatok betöltését az Azure Data Factoryvel"
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: ac7ddaa7-a3a5-4e15-b3cf-c696d2d105df
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
ms.custom: loading
ms.openlocfilehash: 0b01c77c916b616974545fc3da442e72e5336399
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-with-azure-data-factory"></a><span data-ttu-id="331d1-103">Adatok betöltése az Azure Data Factoryvel</span><span class="sxs-lookup"><span data-stu-id="331d1-103">Load Data with Azure Data Factory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="331d1-104">Redgate</span><span class="sxs-lookup"><span data-stu-id="331d1-104">Redgate</span></span>](sql-data-warehouse-load-with-redgate.md)  
> * [<span data-ttu-id="331d1-105">Data Factory</span><span class="sxs-lookup"><span data-stu-id="331d1-105">Data Factory</span></span>](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [<span data-ttu-id="331d1-106">PolyBase</span><span class="sxs-lookup"><span data-stu-id="331d1-106">PolyBase</span></span>](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [<span data-ttu-id="331d1-107">BCP</span><span class="sxs-lookup"><span data-stu-id="331d1-107">BCP</span></span>](sql-data-warehouse-load-with-bcp.md)  
> 
> 

<span data-ttu-id="331d1-108">Ez az oktatóanyag bemutatja, hogyan lehet folyamatokat létrehozni az Azure Data Factoryban az adatok Azure Storage-blobból Azure SQL Data Warehouse-ba való áthelyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="331d1-108">This tutorial shows you how to create a pipeline in Azure Data Factory to move data from Azure Storage Blob to Azure SQL Data Warehouse.</span></span> <span data-ttu-id="331d1-109">A következő lépésekben:</span><span class="sxs-lookup"><span data-stu-id="331d1-109">With the following steps you will:</span></span>

* <span data-ttu-id="331d1-110">Mintaadatokat telepíthet egy Azure Storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="331d1-110">Set up sample data in an Azure Storage Blob.</span></span>
* <span data-ttu-id="331d1-111">Erőforrásokat csatlakoztathat az Azure Data Factoryhoz.</span><span class="sxs-lookup"><span data-stu-id="331d1-111">Connect resources to Azure Data Factory.</span></span>
* <span data-ttu-id="331d1-112">Folyamatot hozhat létre az adatok a tárolóblobokból az SQL Data Warehouse-ba való áthelyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="331d1-112">Create a pipeline to move data from Storage Blobs to SQL Data Warehouse.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a><span data-ttu-id="331d1-113">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="331d1-113">Before you begin</span></span>
<span data-ttu-id="331d1-114">Tekintse át az [Az Azure Data Factory bemutatása][Introduction to Azure Data Factory] című cikket, és ismerje meg az Azure Data Factoryt.</span><span class="sxs-lookup"><span data-stu-id="331d1-114">To familiarize yourself with Azure Data Factory, see [Introduction to Azure Data Factory][Introduction to Azure Data Factory].</span></span>

### <a name="create-or-identify-resources"></a><span data-ttu-id="331d1-115">Erőforrások létrehozása és azonosítása</span><span class="sxs-lookup"><span data-stu-id="331d1-115">Create or identify resources</span></span>
<span data-ttu-id="331d1-116">Az oktatóanyag elindítása előtt rendelkeznie kell a következő erőforrásokkal:</span><span class="sxs-lookup"><span data-stu-id="331d1-116">Before starting this tutorial, you need to have the following resources:</span></span>

* <span data-ttu-id="331d1-117">**Azure Storage-blob**: Az oktatóanyagban az Azure Storage-blob lesz az adatforrás az Azure Data Factory-folyamathoz, így rendelkeznie kell egy elérhető tárolóval a mintaadatok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="331d1-117">**Azure Storage Blob**: This tutorial uses Azure Storage Blob as the data source for the Azure Data Factory pipeline, and so you need to have one available to store the sample data.</span></span> <span data-ttu-id="331d1-118">Ha még nem rendelkezik ilyennel, így hozhat létre: [Tárfiók létrehozása][Create a storage account].</span><span class="sxs-lookup"><span data-stu-id="331d1-118">If you don't have one already, learn how to [Create a storage account][Create a storage account].</span></span>
* <span data-ttu-id="331d1-119">**SQL Data Warehouse**: Az oktatóanyag az adatokat az Azure Storage-blob tárolóból az SQL Data Warehouse-ba helyezi át, ezért rendelkeznie kell egy online adatraktárral, amelybe be vannak töltve az AdventureWorksDW-mintaadatok.</span><span class="sxs-lookup"><span data-stu-id="331d1-119">**SQL Data Warehouse**: This tutorial moves the data from Azure Storage Blob to  SQL Data Warehouse and so need to have a data warehouse online that is loaded with the AdventureWorksDW sample data.</span></span> <span data-ttu-id="331d1-120">Ha nem rendelkezik adattárházzal, sajátítsa el, miként [hozhat létre egyet][Create a SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="331d1-120">If you do not already have a data warehouse, learn how to [provision one][Create a SQL Data Warehouse].</span></span> <span data-ttu-id="331d1-121">Ha rendelkezik adattárházzal, de nem töltötte fel a mintaadatokkal, [manuálisan is feltöltheti azokat][Load sample data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="331d1-121">If you have a data warehouse but didn't provision it with the sample data, you can [load it manually][Load sample data into SQL Data Warehouse].</span></span>
* <span data-ttu-id="331d1-122">**Azure Data Factory**: A tényleges betöltést az Azure Data Factory végzi el, ezért szüksége lesz egyre az adatáthelyezési folyamat elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="331d1-122">**Azure Data Factory**: Azure Data Factory completes the actual load and so you need to have one that you can use to build the data movement pipeline.</span></span> <span data-ttu-id="331d1-123">Ha még nem rendelkezik fiókkal, a [Azure Data Factory (Data Factory Editor) – első lépések][Get started with Azure Data Factory (Data Factory Editor)] 1. lépése leírja, hogyan hozhat létre egyet.</span><span class="sxs-lookup"><span data-stu-id="331d1-123">If you don't have one already, learn how to create one in Step 1 of [Get started with Azure Data Factory (Data Factory Editor)][Get started with Azure Data Factory (Data Factory Editor)].</span></span>
* <span data-ttu-id="331d1-124">**AZCopy**: Az AZCopy a mintaadatok másolásához szükséges a helyi ügyfélről az Azure Storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="331d1-124">**AZCopy**: You need AZCopy to copy the sample data from your local client to your Azure Storage Blob.</span></span> <span data-ttu-id="331d1-125">Telepítési útmutatásért lásd az [AZCopy dokumentációját][AZCopy documentation].</span><span class="sxs-lookup"><span data-stu-id="331d1-125">For install instructions, see the [AZCopy documentation][AZCopy documentation].</span></span>

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a><span data-ttu-id="331d1-126">1. lépés: Mintaadatok másolása az Azure Storage-blobba</span><span class="sxs-lookup"><span data-stu-id="331d1-126">Step 1: Copy sample data to Azure Storage Blob</span></span>
<span data-ttu-id="331d1-127">Ha a fentiek mind rendelkezésre állnak, készen áll a mintaadatok másolására az Azure Storage-blobba.</span><span class="sxs-lookup"><span data-stu-id="331d1-127">Once you have all the pieces ready, you are ready to copy sample data to your Azure Storage Blob.</span></span>

1. <span data-ttu-id="331d1-128">[Mintaadatok letöltése][Download sample data].</span><span class="sxs-lookup"><span data-stu-id="331d1-128">[Download sample data][Download sample data].</span></span> <span data-ttu-id="331d1-129">Ezek az adatok további három év értékesítési adataival bővítik az AdventureWorksDW-mintaadatait.</span><span class="sxs-lookup"><span data-stu-id="331d1-129">This data adds another three years of sales data to your AdventureWorksDW sample data.</span></span>
2. <span data-ttu-id="331d1-130">Töltse be három év adatait az Azure Storage-blobba az AZCopy parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="331d1-130">Use this AZCopy command to copy the three years of data to your Azure Storage Blob.</span></span>
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a><span data-ttu-id="331d1-131">2. lépés: Erőforrások csatlakoztatása az Azure Data Factoryhoz</span><span class="sxs-lookup"><span data-stu-id="331d1-131">Step 2: Connect resources to Azure Data Factory</span></span>
<span data-ttu-id="331d1-132">Most, hogy az adatok a helyükön vannak, létrehozható az Azure Data Factory-folyamat az adatok az Azure Blob Storage-ból az SQL Data Warehouse-ba való áthelyezésére.</span><span class="sxs-lookup"><span data-stu-id="331d1-132">Now that the data is in place we can create the Azure Data Factory pipeline to move the data from Azure blob storage into SQL Data Warehouse.</span></span>

<span data-ttu-id="331d1-133">Első lépésként nyissa meg az [Azure Portal][Azure portal], és válassza ki saját data factoryját a bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="331d1-133">To get started, open the [Azure portal][Azure portal] and select your data factory from the left-hand menu.</span></span>

### <a name="step-21-create-linked-service"></a><span data-ttu-id="331d1-134">2.1. lépés: Társított szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="331d1-134">Step 2.1: Create Linked Service</span></span>
<span data-ttu-id="331d1-135">Társítsa Azure Storage-fiókját és az SQL Data Warehouse-t saját data factoryjához.</span><span class="sxs-lookup"><span data-stu-id="331d1-135">Link your Azure storage account and SQL Data Warehouse to your data factory.</span></span>  

1. <span data-ttu-id="331d1-136">A regisztrációs folyamat elindításához először kattintson a data factory „Összekapcsolt szolgáltatások” szakaszára, majd az „Új adattároló” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="331d1-136">First, begin the registration process by clicking the 'Linked Services' section of your data factory and then click 'New data store.'</span></span> <span data-ttu-id="331d1-137">Válasszon egy nevet, amely alatt az Azure tárterületet regisztrálni szeretné, típusként válassza az Azure Storage lehetőséget, majd adja meg a fióknevet és a fiókkulcsot.</span><span class="sxs-lookup"><span data-stu-id="331d1-137">Choose a name to register your azure storage under, select Azure Storage as your type, and then enter your Account Name and Account Key.</span></span>
2. <span data-ttu-id="331d1-138">Az SQL Data Warehouse regisztrálásához lépjen a „Fejlesztés és üzembe helyezés” szakaszra, és válassza az „Új adattároló”, majd az „Azure SQL Data Warehouse” lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="331d1-138">To register SQL Data Warehouse navigate to the 'Author and Deploy' section, select 'New Data Store', and then 'Azure SQL Data Warehouse'.</span></span> <span data-ttu-id="331d1-139">Másolja és illessze be ezt a sablont, majd adja meg a kért adatokat.</span><span class="sxs-lookup"><span data-stu-id="331d1-139">Copy and paste in this template, and then fill in your specific information.</span></span>
   
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

### <a name="step-22-define-the-dataset"></a><span data-ttu-id="331d1-140">2.2. lépés: Az adatkészlet meghatározása</span><span class="sxs-lookup"><span data-stu-id="331d1-140">Step 2.2: Define the dataset</span></span>
<span data-ttu-id="331d1-141">Az összekapcsolt szolgáltatások létrehozását követően meg kell határoznunk az adatkészleteket.</span><span class="sxs-lookup"><span data-stu-id="331d1-141">After creating the linked services, we will have to define the data sets.</span></span>  <span data-ttu-id="331d1-142">Itt ez a tárolóból az adatbázisba mozgatott adatok struktúrájának meghatározását jelenti.</span><span class="sxs-lookup"><span data-stu-id="331d1-142">Here this means defining the structure of the data that is being moved from your storage to your data warehouse.</span></span>  <span data-ttu-id="331d1-143">További információk a létrehozással kapcsolatban</span><span class="sxs-lookup"><span data-stu-id="331d1-143">You can read more about creating</span></span>

1. <span data-ttu-id="331d1-144">A folyamat indításához lépjen a data factory „Fejlesztés és üzembe helyezés” szakaszára.</span><span class="sxs-lookup"><span data-stu-id="331d1-144">Start this process by navigating to the 'Author and Deploy' section of your data factory.</span></span>
2. <span data-ttu-id="331d1-145">Kattintson az „Új adathalmaz”, majd az „Azure Blob Storage” lehetőségre a tároló a data factoryhoz kapcsolásához.</span><span class="sxs-lookup"><span data-stu-id="331d1-145">Click 'New dataset' and then 'Azure Blob storage' to link your storage to your data factory.</span></span>  <span data-ttu-id="331d1-146">Az alábbi parancsfájl használatával határozhatja meg az adatokat az Azure Blob Storage-ban:</span><span class="sxs-lookup"><span data-stu-id="331d1-146">You can use the below script to define your data in Azure Blob storage:</span></span>
   
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
3. <span data-ttu-id="331d1-147">Most meghatározzuk az adatkészletet az SQL Data Warehouse-hoz.</span><span class="sxs-lookup"><span data-stu-id="331d1-147">Now we define our dataset for SQL Data Warehouse.</span></span> <span data-ttu-id="331d1-148">A folyamat elindításához ismét kattintson az „Új adathalmaz” majd az „Azure SQL Data Warehouse” lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="331d1-148">We start in the same way, by clicking 'New dataset' and then 'Azure SQL Data Warehouse'.</span></span>
   
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

## <a name="step-3-create-and-run-your-pipeline"></a><span data-ttu-id="331d1-149">3. lépés: A folyamat létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="331d1-149">Step 3: Create and run your pipeline</span></span>
<span data-ttu-id="331d1-150">Végül létrehozzuk és futtatjuk a folyamatot az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="331d1-150">Finally, we set up and run the pipeline in Azure Data Factory.</span></span>  <span data-ttu-id="331d1-151">Ez a művelet végzi el az adatok tényleges áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="331d1-151">This is the operation that completes the actual data movement.</span></span>  <span data-ttu-id="331d1-152">Az SQL Data Warehouse-ban és az Azure Data Factoryben végrehajtható műveletek teljes listáját [itt] találja[Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="331d1-152">You can find a full view of the operations that you can complete with SQL Data Warehouse and Azure Data Factory [here][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].</span></span>

<span data-ttu-id="331d1-153">A „Fejlesztés és üzembe helyezés” szakaszban kattintson a „Több parancs”, majd az „Új adatcsatorna” elemre.</span><span class="sxs-lookup"><span data-stu-id="331d1-153">In the 'Author and Deploy' section, click 'More Commands' and then 'New Pipeline'.</span></span>  <span data-ttu-id="331d1-154">Miután létrehozta a folyamatot, az alábbi kód használatával helyezheti át az adatokat az adatraktárba:</span><span class="sxs-lookup"><span data-stu-id="331d1-154">After you create the pipeline, you can use the below code to transfer the data to your data warehouse:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="331d1-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="331d1-155">Next steps</span></span>
<span data-ttu-id="331d1-156">Ha többet szeretne tudni, kezdje a következők áttekintésével:</span><span class="sxs-lookup"><span data-stu-id="331d1-156">To learn more, start by viewing:</span></span>

* <span data-ttu-id="331d1-157">[Azure Data Factory képzési terv][Azure Data Factory learning path].</span><span class="sxs-lookup"><span data-stu-id="331d1-157">[Azure Data Factory learning path][Azure Data Factory learning path].</span></span>
* <span data-ttu-id="331d1-158">[Azure SQL Data Warehouse-összekötő][Azure SQL Data Warehouse Connector].</span><span class="sxs-lookup"><span data-stu-id="331d1-158">[Azure SQL Data Warehouse Connector][Azure SQL Data Warehouse Connector].</span></span> <span data-ttu-id="331d1-159">Ez a fő referencia-témakör az Azure Data Factory és az Azure SQL Data Warehouse együttes használatáról.</span><span class="sxs-lookup"><span data-stu-id="331d1-159">This is the core reference topic for using Azure Data Factory with Azure SQL Data Warehouse.</span></span>

<span data-ttu-id="331d1-160">Ezek a témakörök további információkat adnak az Azure Data Factory szolgáltatással kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="331d1-160">These topics provide detailed information about Azure Data Factory.</span></span> <span data-ttu-id="331d1-161">Ugyan az Azure SQL Database és a HDInsight a témájuk, a bennük foglalt információk azonban az Azure SQL Data Warehouse-ra is vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="331d1-161">They discuss Azure SQL Database or HDInsight, but the information also applies to Azure SQL Data Warehouse.</span></span>

* <span data-ttu-id="331d1-162">[Oktatóanyag: Azure Data Factory – első lépések][Tutorial: Get started with Azure Data Factory] Ez a fő oktatóanyag az adatok Azure Data Factoryval történő feldolgozásáról.</span><span class="sxs-lookup"><span data-stu-id="331d1-162">[Tutorial: Get started with Azure Data Factory][Tutorial: Get started with Azure Data Factory] This is the core tutorial for processing data with Azure Data Factory.</span></span> <span data-ttu-id="331d1-163">Ebben az oktatóanyagban felépítheti első folyamatát, amely a HDInsight használatával webes naplókat alakít át és elemez havi rendszerességgel.</span><span class="sxs-lookup"><span data-stu-id="331d1-163">In this tutorial, you will build your first pipeline that uses HDInsight to transform and analyze web logs on a monthly basis.</span></span> <span data-ttu-id="331d1-164">Megjegyzés: Ebben az oktatóanyagban nincs másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="331d1-164">Note, there is no copy activity in this tutorial.</span></span>
* <span data-ttu-id="331d1-165">[Oktatóanyag: Adatok másolása az Azure Storage-blobból az Azure SQL Database-be][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span><span class="sxs-lookup"><span data-stu-id="331d1-165">[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database].</span></span> <span data-ttu-id="331d1-166">Ebben az oktatóanyagban létrehozhat egy folyamatot az Azure Data Factoryben az adatok Azure Storage-blobból az Azure SQL Database-be való áthelyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="331d1-166">In this tutorial, you create a pipeline in Azure Data Factory to copy data from Azure Storage Blob to Azure SQL Database.</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction to Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv
