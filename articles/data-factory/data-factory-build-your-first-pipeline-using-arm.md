---
title: "Az első data factory létrehozása (Resource Manager-sablon) | Microsoft Docs"
description: "Ebben az oktatóprogramban egy egyszerű minta Azure Data Factory-folyamatot fog létrehozni egy Azure Resource Manager-sablon segítségével."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: eb9e70b9-a13a-4a27-8256-2759496be470
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: c67169f296f2f13b9ee87180f126fb1dcf10fbea
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a><span data-ttu-id="336bb-103">Oktatóanyag: Az első Azure data factory létrehozása Azure Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="336bb-103">Tutorial: Build your first Azure data factory using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="336bb-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="336bb-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="336bb-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="336bb-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="336bb-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="336bb-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="336bb-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="336bb-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="336bb-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="336bb-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="336bb-109">REST API</span><span class="sxs-lookup"><span data-stu-id="336bb-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

<span data-ttu-id="336bb-110">Ez a cikk bemutatja, hogyan hozhatja létre első Azure data factoryját egy Azure Resource Manager-sablonnal.</span><span class="sxs-lookup"><span data-stu-id="336bb-110">In this article, you use an Azure Resource Manager template to create your first Azure data factory.</span></span> <span data-ttu-id="336bb-111">Ha ezt az oktatóanyagot más eszközök/SDK-k használatával szeretné elvégezni, válassza ki az egyik lehetőséget a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="336bb-111">To do the tutorial using other tools/SDKs, select one of the options from the drop-down list.</span></span>

<span data-ttu-id="336bb-112">A jelen oktatóanyagban szereplő folyamat egyetlen tevékenységet tartalmaz: ez a **HDInsight Hive-tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="336bb-112">The pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="336bb-113">A tevékenység egy hive-szkriptet futtat egy Azure HDInsight fürtön, amely a bemeneti adatokat átalakítja a kimeneti adatok előállításához.</span><span class="sxs-lookup"><span data-stu-id="336bb-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data to produce output data.</span></span> <span data-ttu-id="336bb-114">A folyamat úgy van ütemezve, hogy havonta egyszer fusson a megadott kezdő és befejező időpontok közt.</span><span class="sxs-lookup"><span data-stu-id="336bb-114">The pipeline is scheduled to run once a month between the specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="336bb-115">Az oktatóanyagban található adatfolyamat átalakítja a bemeneti adatokat, hogy ezzel kimeneti adatokat hozzon létre.</span><span class="sxs-lookup"><span data-stu-id="336bb-115">The data pipeline in this tutorial transforms input data to produce output data.</span></span> <span data-ttu-id="336bb-116">Az adatok Azure Data Factory használatával történő másolásának útmutatásáért olvassa el [az adatok Blob Storage-ból SQL Database-be történő másolását ismertető oktatóanyagot](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="336bb-116">For a tutorial on how to copy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage to SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="336bb-117">Az oktatóanyagban szereplő folyamat csak egyetlen tevékenységtípussal rendelkezik: HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="336bb-117">The pipeline in this tutorial has only one activity of type: HDInsightHive.</span></span> <span data-ttu-id="336bb-118">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="336bb-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="336bb-119">Ezenkívül össze is fűzhet két tevékenységet (egymás után futtathatja őket), ha az egyik tevékenység kimeneti adatkészletét a másik tevékenység bemeneti adatkészleteként állítja be.</span><span class="sxs-lookup"><span data-stu-id="336bb-119">And, you can chain two activities (run one activity after another) by setting the output dataset of one activity as the input dataset of the other activity.</span></span> <span data-ttu-id="336bb-120">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="336bb-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="336bb-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="336bb-121">Prerequisites</span></span>
* <span data-ttu-id="336bb-122">Olvassa el [Az oktatóanyag áttekintése](data-factory-build-your-first-pipeline.md) című részt, és hajtsa végre az **előfeltételként** felsorolt lépéseket.</span><span class="sxs-lookup"><span data-stu-id="336bb-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete the **prerequisite** steps.</span></span>
* <span data-ttu-id="336bb-123">Kövesse a [How to install and configure Azure PowerShell](/powershell/azure/overview) (Az Azure PowerShell telepítése és konfigurálása) című cikkben foglalt utasításokat az Azure PowerShell telepítéséhez a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="336bb-123">Follow instructions in [How to install and configure Azure PowerShell](/powershell/azure/overview) article to install latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="336bb-124">Az Azure Resource Manager-sablonokkal kapcsolatban az [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) (Azure Resource Manager-sablonok készítése) című cikkben tájékozódhat bővebben.</span><span class="sxs-lookup"><span data-stu-id="336bb-124">See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) to learn about Azure Resource Manager templates.</span></span> 

## <a name="in-this-tutorial"></a><span data-ttu-id="336bb-125">Az oktatóanyag tartalma</span><span class="sxs-lookup"><span data-stu-id="336bb-125">In this tutorial</span></span>
| <span data-ttu-id="336bb-126">Entitás</span><span class="sxs-lookup"><span data-stu-id="336bb-126">Entity</span></span> | <span data-ttu-id="336bb-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="336bb-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="336bb-128">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-128">Azure Storage linked service</span></span> |<span data-ttu-id="336bb-129">Társítja az Azure Storage-fiókot a data factoryhoz.</span><span class="sxs-lookup"><span data-stu-id="336bb-129">Links your Azure Storage account to the data factory.</span></span> <span data-ttu-id="336bb-130">Ebben a példában az Azure Storage-fiók a bemeneti és a kimeneti adatokat tárolja a folyamathoz.</span><span class="sxs-lookup"><span data-stu-id="336bb-130">The Azure Storage account holds the input and output data for the pipeline in this sample.</span></span> |
| <span data-ttu-id="336bb-131">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-131">HDInsight on-demand linked service</span></span> |<span data-ttu-id="336bb-132">Egy igény szerinti HDInsight-fürtöt társít a data factoryhoz.</span><span class="sxs-lookup"><span data-stu-id="336bb-132">Links an on-demand HDInsight cluster to the data factory.</span></span> <span data-ttu-id="336bb-133">A rendszer automatikusan létrehozza a fürtöt az adatok feldolgozásához, majd törli a feldolgozás befejezése után.</span><span class="sxs-lookup"><span data-stu-id="336bb-133">The cluster is automatically created for you to process data and is deleted after the processing is done.</span></span> |
| <span data-ttu-id="336bb-134">Azure Blob bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-134">Azure Blob input dataset</span></span> |<span data-ttu-id="336bb-135">Az Azure Storage társított szolgáltatásra vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="336bb-135">Refers to the Azure Storage linked service.</span></span> <span data-ttu-id="336bb-136">A társított szolgáltatás egy Azure Storage-fiókra hivatkozik, az Azure-blob adatkészlet pedig meghatározza a bemeneti adatokat tartalmazó tárban lévő tárolót, mappát és fájlnevet.</span><span class="sxs-lookup"><span data-stu-id="336bb-136">The linked service refers to an Azure Storage account and the Azure Blob dataset specifies the container, folder, and file name in the storage that holds the input data.</span></span> |
| <span data-ttu-id="336bb-137">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-137">Azure Blob output dataset</span></span> |<span data-ttu-id="336bb-138">Az Azure Storage társított szolgáltatásra vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="336bb-138">Refers to the Azure Storage linked service.</span></span> <span data-ttu-id="336bb-139">A társított szolgáltatás egy Azure Storage-fiókra hivatkozik, az Azure-blob adatkészlet pedig meghatározza a kimeneti adatokat tartalmazó tárban lévő tárolót, mappát és fájlnevet.</span><span class="sxs-lookup"><span data-stu-id="336bb-139">The linked service refers to an Azure Storage account and the Azure Blob dataset specifies the container, folder, and file name in the storage that holds the output data.</span></span> |
| <span data-ttu-id="336bb-140">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="336bb-140">Data pipeline</span></span> |<span data-ttu-id="336bb-141">A folyamat egyetlen HDInsightHive típusú tevékenységgel rendelkezik, amely felhasználja a beérkező adatkészletet, és elkészíti a kimenőt.</span><span class="sxs-lookup"><span data-stu-id="336bb-141">The pipeline has one activity of type HDInsightHive, which consumes the input dataset and produces the output dataset.</span></span> |

<span data-ttu-id="336bb-142">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="336bb-142">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="336bb-143">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="336bb-143">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="336bb-144">Kétféle típusú tevékenység létezik: az [adattovábbítási tevékenységek](data-factory-data-movement-activities.md) és az [adatátalakítási tevékenységek](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="336bb-144">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="336bb-145">Az oktatóanyag segítségével egyetlen tevékenységgel (Hive-tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="336bb-145">In this tutorial, you create a pipeline with one activity (Hive activity).</span></span>

<span data-ttu-id="336bb-146">A következő szakasz a Data Factory-entitások meghatározására szolgáló teljes Resource Manager-sablont ismerteti, így gyorsan végighaladhat az oktatóanyagon és tesztelheti a sablont.</span><span class="sxs-lookup"><span data-stu-id="336bb-146">The following section provides the complete Resource Manager template for defining Data Factory entities so that you can quickly run through the tutorial and test the template.</span></span> <span data-ttu-id="336bb-147">Az egyes Data Factory-entitások meghatározásának megértéséhez tekintse meg a [Data Factory-entitások a sablonban](#data-factory-entities-in-the-template) szakaszt.</span><span class="sxs-lookup"><span data-stu-id="336bb-147">To understand how each Data Factory entity is defined, see [Data Factory entities in the template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="336bb-148">Data Factory JSON-sablon</span><span class="sxs-lookup"><span data-stu-id="336bb-148">Data Factory JSON template</span></span>
<span data-ttu-id="336bb-149">A data factory meghatározásához szükséges legfelső szintű Resource Manager-sablon a következő:</span><span class="sxs-lookup"><span data-stu-id="336bb-149">The top-level Resource Manager template for defining a data factory is:</span></span> 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```
<span data-ttu-id="336bb-150">Hozzon létre egy **ADFTutorialARM.json** nevű JSON-fájlt a **C:\ADFGetStarted** mappában az alábbi tartalommal:</span><span class="sxs-lookup"><span data-stu-id="336bb-150">Create a JSON file named **ADFTutorialARM.json** in **C:\ADFGetStarted** folder with the following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the input/output data." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
          "blobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
          "inputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that has the input file." } },
          "inputBlobName": { "type": "string", "metadata": { "description": "Name of the input file/blob." } },
          "outputBlobFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that will hold the transformed data." } },
          "hiveScriptFolder": { "type": "string", "metadata": { "description": "The folder in the blob container that contains the Hive query file." } },
          "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of the hive query (HQL) file." } }
    },
    "variables": {
          "dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
          "azureStorageLinkedServiceName": "AzureStorageLinkedService",
          "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
          "blobInputDatasetName": "AzureBlobInput",
          "blobOutputDatasetName": "AzureBlobOutput",
          "pipelineName": "HiveTransformPipeline"
    },
    "resources": [
      {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US",
        "resources": [
          {
            "type": "linkedservices",
            "name": "[variables('azureStorageLinkedServiceName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureStorage",
                  "description": "Azure Storage linked service",
                  "typeProperties": {
                    "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
                  }
            }
          },
          {
            "type": "linkedservices",
            "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "HDInsightOnDemand",
                  "typeProperties": {
                    "version": "3.5",
                    "clusterSize": 1,
                    "timeToLive": "00:05:00",
                    "osType": "Linux",
                    "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
                  }
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobInputDatasetName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "typeProperties": {
                    "fileName": "[parameters('inputBlobName')]",
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
                    "format": {
                          "type": "TextFormat",
                          "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Month",
                    "interval": 1
                  },
                  "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobOutputDatasetName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "type": "AzureBlob",
                  "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
                  "typeProperties": {
                    "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
                    "format": {
                          "type": "TextFormat",
                          "columnDelimiter": ","
                    }
                  },
                  "availability": {
                    "frequency": "Month",
                    "interval": 1
                  }
            }
          },
          {
            "type": "datapipelines",
            "name": "[variables('pipelineName')]",
            "dependsOn": [
                  "[variables('dataFactoryName')]",
                  "[variables('azureStorageLinkedServiceName')]",
                  "[variables('hdInsightOnDemandLinkedServiceName')]",
                  "[variables('blobInputDatasetName')]",
                  "[variables('blobOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
                  "description": "Pipeline that transforms data using Hive script.",
                  "activities": [
                {
                      "type": "HDInsightHive",
                      "typeProperties": {
                        "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                        "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                        "defines": {
                              "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                              "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                        }
                      },
                      "inputs": [
                        {
                              "name": "[variables('blobInputDatasetName')]"
                        }
                      ],
                      "outputs": [
                        {
                              "name": "[variables('blobOutputDatasetName')]"
                        }
                      ],
                      "policy": {
                        "concurrency": 1,
                        "retry": 3
                      },
                      "scheduler": {
                        "frequency": "Month",
                        "interval": 1
                      },
                      "name": "RunSampleHiveActivity",
                      "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
                }
                  ],
                  "start": "2017-07-01T00:00:00Z",
                  "end": "2017-07-02T00:00:00Z",
                  "isPaused": false
              }
          }
        ]
      }
    ]
}
```

> [!NOTE]
> <span data-ttu-id="336bb-151">Az Azure data factory létrehozására szolgáló Resource Manager-sablonra a következő helyen találhat egy másik példát: [Oktatóanyag: Folyamat létrehozása másolási tevékenységgel és Azure Resource Manager-sablon használatával](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="336bb-151">You can find another example of Resource Manager template for creating an Azure data factory on [Tutorial: Create a pipeline with Copy Activity using an Azure Resource Manager template](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span></span>  
> 
> 

## <a name="parameters-json"></a><span data-ttu-id="336bb-152">Paramétereket tartalmazó JSON-file</span><span class="sxs-lookup"><span data-stu-id="336bb-152">Parameters JSON</span></span>
<span data-ttu-id="336bb-153">Hozzon létre egy **ADFTutorialARM-Parameters.json** elnevezésű JSON-fájlt, amely paramétereket tartalmaz az Azure Resource Manager-sablon számára.</span><span class="sxs-lookup"><span data-stu-id="336bb-153">Create a JSON file named **ADFTutorialARM-Parameters.json** that contains parameters for the Azure Resource Manager template.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="336bb-154">Adja meg az Azure Storage-fiók nevét és kulcsát a **storageAccountName** és **storageAccountKey** paraméterek értékeinek ebben a paraméterfájlban.</span><span class="sxs-lookup"><span data-stu-id="336bb-154">Specify the name and key of your Azure Storage account for the **storageAccountName** and **storageAccountKey** parameters in this parameter file.</span></span> 
> 
> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "storageAccountName": {
            "value": "<Name of your Azure Storage account>"
        },
        "storageAccountKey": {
            "value": "<Key of your Azure Storage account>"
        },
        "blobContainer": {
            "value": "adfgetstarted"
        },
        "inputBlobFolder": {
            "value": "inputdata"
        },
        "inputBlobName": {
            "value": "input.log"
        },
        "outputBlobFolder": {
            "value": "partitioneddata"
        },
        "hiveScriptFolder": {
              "value": "script"
        },
        "hiveScriptFile": {
              "value": "partitionweblogs.hql"
        }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="336bb-155">Készíthet különálló JSON-paraméterfájlokat a fejlesztési, tesztelési és az éles környezetek számára, amelyeket ugyanazzal a Data Factory JSON-sablonnal használhat.</span><span class="sxs-lookup"><span data-stu-id="336bb-155">You may have separate parameter JSON files for development, testing, and production environments that you can use with the same Data Factory JSON template.</span></span> <span data-ttu-id="336bb-156">Ezekben a környezetekben automatizálhatja a Data Factory-entitások üzembe helyezését egy PowerShell-szkript használatával.</span><span class="sxs-lookup"><span data-stu-id="336bb-156">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span> 
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="336bb-157">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="336bb-157">Create data factory</span></span>
1. <span data-ttu-id="336bb-158">Indítsa el az **Azure PowerShellt**, és futtassa az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="336bb-158">Start **Azure PowerShell** and run the following command:</span></span> 
   * <span data-ttu-id="336bb-159">Futtassa a következő parancsot, és adja meg az Azure Portalra való bejelentkezéshez használt felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="336bb-159">Run the following command and enter the user name and password that you use to sign in to the Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * <span data-ttu-id="336bb-160">Futtassa a következő parancsot a fiókhoz tartozó előfizetések megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="336bb-160">Run the following command to view all the subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * <span data-ttu-id="336bb-161">Futtassa a következő parancsot a használni kívánt előfizetés kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="336bb-161">Run the following command to select the subscription that you want to work with.</span></span> <span data-ttu-id="336bb-162">Ennek az előfizetésnek egyeznie kell az Azure Portalon használt előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="336bb-162">This subscription should be the same as the one you used in the Azure portal.</span></span>
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. <span data-ttu-id="336bb-163">Futtassa a következő parancsot a Data Factory-entitásoknak az 1. lépésben létrehozott Resource Manager-sablonnal történő telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="336bb-163">Run the following command to deploy Data Factory entities using the Resource Manager template you created in Step 1.</span></span> 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="336bb-164">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="336bb-164">Monitor pipeline</span></span>
1. <span data-ttu-id="336bb-165">Miután bejelentkezett az [Azure Portalra](https://portal.azure.com/), kattintson a **Tallózás** elemre, és válassza az **Adat-előállítók** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="336bb-165">After logging in to the [Azure portal](https://portal.azure.com/), Click **Browse** and select **Data factories**.</span></span>
     <span data-ttu-id="336bb-166">![Tallózás->Data Factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span><span class="sxs-lookup"><span data-stu-id="336bb-166">![Browse->Data factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span></span>
2. <span data-ttu-id="336bb-167">A **Data Factories** panelen kattintson a létrehozott data factoryre (**TutorialFactoryARM**).</span><span class="sxs-lookup"><span data-stu-id="336bb-167">In the **Data Factories** blade, click the data factory (**TutorialFactoryARM**) you created.</span></span>    
3. <span data-ttu-id="336bb-168">A data factoryhoz tartozó **Data Factory** panelen kattintson a **Diagram** elemre.</span><span class="sxs-lookup"><span data-stu-id="336bb-168">In the **Data Factory** blade for your data factory, click **Diagram**.</span></span>

     ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. <span data-ttu-id="336bb-170">A **diagramnézet** áttekintést nyújt az oktatóanyagban használt folyamatokról és adatkészletekről.</span><span class="sxs-lookup"><span data-stu-id="336bb-170">In the **Diagram View**, you see an overview of the pipelines, and datasets used in this tutorial.</span></span>
   
   ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. <span data-ttu-id="336bb-172">A diagramnézetben kattintson duplán az **AzureBlobOutput** adatkészletre.</span><span class="sxs-lookup"><span data-stu-id="336bb-172">In the Diagram View, double-click the dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="336bb-173">Látni fogja, hogy a szelet feldolgozás alatt áll.</span><span class="sxs-lookup"><span data-stu-id="336bb-173">You see that the slice that is currently being processed.</span></span>
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. <span data-ttu-id="336bb-175">A feldolgozás befejeztével a szelet **Ready** (Kész) állapotúra vált.</span><span class="sxs-lookup"><span data-stu-id="336bb-175">When processing is done, you see the slice in **Ready** state.</span></span> <span data-ttu-id="336bb-176">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="336bb-176">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="336bb-177">Ezért a folyamat várhatóan **körülbelül 30 perc** alatt dolgozza fel a szeletet.</span><span class="sxs-lookup"><span data-stu-id="336bb-177">Therefore, expect the pipeline to take **approximately 30 minutes** to process the slice.</span></span>
   
    ![Adathalmaz](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. <span data-ttu-id="336bb-179">Ha a szelet **Ready** (Kész) állapotú, a kimeneti adatok **adfgetstarted** Blob Storage-tárolójában ellenőrizze a **partitioneddata** mappát.</span><span class="sxs-lookup"><span data-stu-id="336bb-179">When the slice is in **Ready** state, check the **partitioneddata** folder in the **adfgetstarted** container in your blob storage for the output data.</span></span>  

<span data-ttu-id="336bb-180">A [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) (Adatkészletek és folyamatok figyelése) című cikkben útmutatást találhat arról, hogy hogyan használhatja az Azure Portal paneleit az oktatóanyagban létrehozott folyamatok és adatkészletek figyeléséhez.</span><span class="sxs-lookup"><span data-stu-id="336bb-180">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how to use the Azure portal blades to monitor the pipeline and datasets you have created in this tutorial.</span></span>

<span data-ttu-id="336bb-181">Az adatfolyamatok figyeléséhez a Monitor and Manage Appot is használhatja.</span><span class="sxs-lookup"><span data-stu-id="336bb-181">You can also use Monitor and Manage App to monitor your data pipelines.</span></span> <span data-ttu-id="336bb-182">Az alkalmazás használatával kapcsolatos információkért tekintse meg a [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) (Azure Data Factory-folyamatok figyelése és felügyelete a Monitoring App használatával) című cikket.</span><span class="sxs-lookup"><span data-stu-id="336bb-182">See [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) for details about using the application.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="336bb-183">A szelet sikeres feldolgozásakor a rendszer törli a bemeneti fájlt.</span><span class="sxs-lookup"><span data-stu-id="336bb-183">The input file gets deleted when the slice is processed successfully.</span></span> <span data-ttu-id="336bb-184">Ezért ha újra le szeretné futtatni a szeletet, vagy újra el szeretné végezni az oktatóanyagban foglaltakat, töltse fel a bemeneti fájlt (input.log) az adfgetstarted tároló inputdata mappájába.</span><span class="sxs-lookup"><span data-stu-id="336bb-184">Therefore, if you want to rerun the slice or do the tutorial again, upload the input file (input.log) to the inputdata folder of the adfgetstarted container.</span></span>
> 
> 

## <a name="data-factory-entities-in-the-template"></a><span data-ttu-id="336bb-185">Data Factory-entitások a sablonban</span><span class="sxs-lookup"><span data-stu-id="336bb-185">Data Factory entities in the template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="336bb-186">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="336bb-186">Define data factory</span></span>
<span data-ttu-id="336bb-187">A data factoryt a Resource Manager-sablonban definiálhatja az alábbi minta szerint:</span><span class="sxs-lookup"><span data-stu-id="336bb-187">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
<span data-ttu-id="336bb-188">A dataFactoryName az alábbi módon van definiálva:</span><span class="sxs-lookup"><span data-stu-id="336bb-188">The dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
<span data-ttu-id="336bb-189">Ez az erőforráscsoport-azonosítón alapuló egyedi karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="336bb-189">It is a unique string based on the resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="336bb-190">Data Factory-entitások definiálása</span><span class="sxs-lookup"><span data-stu-id="336bb-190">Defining Data Factory entities</span></span>
<span data-ttu-id="336bb-191">Az alábbi Data Factory-entitások a JSON-sablonban vannak definiálva:</span><span class="sxs-lookup"><span data-stu-id="336bb-191">The following Data Factory entities are defined in the JSON template:</span></span> 

* [<span data-ttu-id="336bb-192">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-192">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="336bb-193">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-193">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="336bb-194">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-194">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="336bb-195">Azure blobkimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-195">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="336bb-196">Másolási tevékenységgel rendelkező adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="336bb-196">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="336bb-197">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-197">Azure Storage linked service</span></span>
<span data-ttu-id="336bb-198">Ebben a szakaszban megadhatja az Azure-tárfiók nevét és kulcsát.</span><span class="sxs-lookup"><span data-stu-id="336bb-198">You specify the name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="336bb-199">Az Azure Storage társított szolgáltatás definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Storage társított szolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="336bb-199">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used to define an Azure Storage linked service.</span></span> 

```json
{
    "type": "linkedservices",
    "name": "[variables('azureStorageLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureStorage",
        "description": "Azure Storage linked service",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
<span data-ttu-id="336bb-200">A **connectionString** a storageAccountName és storageAccountKey paramétereket használja.</span><span class="sxs-lookup"><span data-stu-id="336bb-200">The **connectionString** uses the storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="336bb-201">A paraméterek értékei a konfigurációs fájlok használatával adhatók át.</span><span class="sxs-lookup"><span data-stu-id="336bb-201">The values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="336bb-202">A definíció változókat is használ: azureStroageLinkedService és dataFactoryName, amelyek a sablonban vannak definiálva.</span><span class="sxs-lookup"><span data-stu-id="336bb-202">The definition also uses variables: azureStroageLinkedService and dataFactoryName defined in the template.</span></span> 

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="336bb-203">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="336bb-203">HDInsight on-demand linked service</span></span>
<span data-ttu-id="336bb-204">A HDInsight igény szerinti társított szolgáltatás definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg a [Számítási társított szolgáltatás](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) című cikket.</span><span class="sxs-lookup"><span data-stu-id="336bb-204">See [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used to define an HDInsight on-demand linked service.</span></span>  

```json
{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "[variables('azureStorageLinkedServiceName')]"
        }
    }
}
```
<span data-ttu-id="336bb-205">Vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="336bb-205">Note the following points:</span></span> 

* <span data-ttu-id="336bb-206">A Data Factory létrehoz egy **Linux-alapú** HDInsight-fürtöt a fenti JSON-fájllal.</span><span class="sxs-lookup"><span data-stu-id="336bb-206">The Data Factory creates a **Linux-based** HDInsight cluster for you with the above JSON.</span></span> <span data-ttu-id="336bb-207">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="336bb-207">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span> 
* <span data-ttu-id="336bb-208">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="336bb-208">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="336bb-209">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="336bb-209">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="336bb-210">A HDInsight-fürt létrehoz egy **alapértelmezett tárolót** a JSON-fájlban megadott blob-tárolóban (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="336bb-210">The HDInsight cluster creates a **default container** in the blob storage you specified in the JSON (**linkedServiceName**).</span></span> <span data-ttu-id="336bb-211">A fürt törlésekor a HDInsight nem törli ezt a tárolót.</span><span class="sxs-lookup"><span data-stu-id="336bb-211">HDInsight does not delete this container when the cluster is deleted.</span></span> <span data-ttu-id="336bb-212">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="336bb-212">This behavior is by design.</span></span> <span data-ttu-id="336bb-213">Igény szerinti HDInsight társított szolgáltatás esetén a rendszer mindig létrehoz egy HDInsight-fürt, amikor fel kell dolgozni egy szeletet, kivéve, ha van meglévő élő fürt (**timeToLive**), majd a feldolgozás végén a rendszer törli a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="336bb-213">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs to be processed unless there is an existing live cluster (**timeToLive**) and is deleted when the processing is done.</span></span>
  
    <span data-ttu-id="336bb-214">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="336bb-214">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="336bb-215">Ha nincs szüksége rájuk a feladatokkal kapcsolatos hibaelhárításhoz, törölheti őket a tárolási költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="336bb-215">If you do not need them for troubleshooting of the jobs, you may want to delete them to reduce the storage cost.</span></span> <span data-ttu-id="336bb-216">A tárolók neve a következő mintát követi: „adf**yourdatafactoryname**-**linkedservicename**-datetimestamp”.</span><span class="sxs-lookup"><span data-stu-id="336bb-216">The names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="336bb-217">Az Azure Blob Storage-tárból olyan eszközökkel törölheti a tárolókat, mint például a [Microsoft Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="336bb-217">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) to delete containers in your Azure blob storage.</span></span>

<span data-ttu-id="336bb-218">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="336bb-218">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="336bb-219">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-219">Azure blob input dataset</span></span>
<span data-ttu-id="336bb-220">Megadhatja a bemeneti adatokat tartalmazó blobtároló, mappa és fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="336bb-220">You specify the names of blob container, folder, and file that contains the input data.</span></span> <span data-ttu-id="336bb-221">Az Azure Blob-adatkészletek definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Blob-adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="336bb-221">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span> 

```json
{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "typeProperties": {
            "fileName": "[parameters('inputBlobName')]",
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('inputBlobFolder'))]",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        },
        "external": true
    }
}
```
<span data-ttu-id="336bb-222">Ez a definíció az alábbi, a paramétersablonban definiált paramétereket használja: blobContainer, inputBlobFolder és inputBlobName.</span><span class="sxs-lookup"><span data-stu-id="336bb-222">This definition uses the following parameters defined in parameter template: blobContainer, inputBlobFolder, and inputBlobName.</span></span> 

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="336bb-223">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="336bb-223">Azure Blob output dataset</span></span>
<span data-ttu-id="336bb-224">Megadhatja a kimeneti adatokat tartalmazó blobtároló és mappa nevét.</span><span class="sxs-lookup"><span data-stu-id="336bb-224">You specify the names of blob container and folder that holds the output data.</span></span> <span data-ttu-id="336bb-225">Az Azure Blob-adatkészletek definiálásához használt JSON-tulajdonságokkal kapcsolatos információkért tekintse meg az [Azure Blob-adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="336bb-225">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used to define an Azure Blob dataset.</span></span>  

```json
{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "[concat(parameters('blobContainer'), '/', parameters('outputBlobFolder'))]",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1
        }
    }
}
```

<span data-ttu-id="336bb-226">Ez a definíció az alábbi, a paramétersablonban definiált paramétereket használja: blobContainer és outputBlobFolder.</span><span class="sxs-lookup"><span data-stu-id="336bb-226">This definition uses the following parameters defined in the parameter template: blobContainer and outputBlobFolder.</span></span> 

#### <a name="data-pipeline"></a><span data-ttu-id="336bb-227">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="336bb-227">Data pipeline</span></span>
<span data-ttu-id="336bb-228">Definiálhat egy folyamatot, amely átalakítja az adatokat a Hive-parancsfájl egy igény szerinti Azure HDInsight-fürtön való futtatásával.</span><span class="sxs-lookup"><span data-stu-id="336bb-228">You define a pipeline that transform data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="336bb-229">A példában található folyamat definiálásához használt JSON-elemek leírásához tekintse meg [A folyamat JSON-fájlja](data-factory-create-pipelines.md#pipeline-json) című szakaszt.</span><span class="sxs-lookup"><span data-stu-id="336bb-229">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used to define a pipeline in this example.</span></span> 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
        "[variables('azureStorageLinkedServiceName')]",
        "[variables('hdInsightOnDemandLinkedServiceName')]",
        "[variables('blobInputDatasetName')]",
        "[variables('blobOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "description": "Pipeline that transforms data using Hive script.",
        "activities": [
        {
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "[concat(parameters('blobContainer'), '/', parameters('hiveScriptFolder'), '/', parameters('hiveScriptFile'))]",
                "scriptLinkedService": "[variables('azureStorageLinkedServiceName')]",
                "defines": {
                    "inputtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('inputBlobFolder'))]",
                    "partitionedtable": "[concat('wasb://', parameters('blobContainer'), '@', parameters('storageAccountName'), '.blob.core.windows.net/', parameters('outputBlobFolder'))]"
                }
            },
            "inputs": [
            {
                "name": "[variables('blobInputDatasetName')]"
            }
            ],
            "outputs": [
            {
                "name": "[variables('blobOutputDatasetName')]"
            }
            ],
            "policy": {
                "concurrency": 1,
                "retry": 3
            },
            "scheduler": {
                "frequency": "Month",
                "interval": 1
            },
            "name": "RunSampleHiveActivity",
            "linkedServiceName": "[variables('hdInsightOnDemandLinkedServiceName')]"
        }
        ],
        "start": "2017-07-01T00:00:00Z",
        "end": "2017-07-02T00:00:00Z",
        "isPaused": false
    }
}
```

## <a name="reuse-the-template"></a><span data-ttu-id="336bb-230">A sablon ismételt felhasználása</span><span class="sxs-lookup"><span data-stu-id="336bb-230">Reuse the template</span></span>
<span data-ttu-id="336bb-231">Az oktatóanyagban létrehozott egy sablont a Data Factory-entitások definiálásához, illetve egy másikat a paraméterek értékeinek átadásához.</span><span class="sxs-lookup"><span data-stu-id="336bb-231">In the tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="336bb-232">Ha ugyanazt a sablont szeretné használni a Data Factory-entitások különböző környezetekben történő üzembe helyezéséhez, hozzon létre egy paraméterfájlt az egyes környezetekhez, és használja azt az adott környezetben történő üzembe helyezéskor.</span><span class="sxs-lookup"><span data-stu-id="336bb-232">To use the same template to deploy Data Factory entities to different environments, you create a parameter file for each environment and use it when deploying to that environment.</span></span>     

<span data-ttu-id="336bb-233">Példa:</span><span class="sxs-lookup"><span data-stu-id="336bb-233">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
<span data-ttu-id="336bb-234">Megfigyelheti, hogy az első parancs a fejlesztőkörnyezet, a második a tesztkörnyezet, a harmadik pedig az éles környezet paraméterfájlját használja.</span><span class="sxs-lookup"><span data-stu-id="336bb-234">Notice that the first command uses parameter file for the development environment, second one for the test environment, and the third one for the production environment.</span></span>  

<span data-ttu-id="336bb-235">Emellett ismétlődő feladatok elvégzéséhez is újból felhasználhatja a sablont.</span><span class="sxs-lookup"><span data-stu-id="336bb-235">You can also reuse the template to perform repeated tasks.</span></span> <span data-ttu-id="336bb-236">Ilyen eset például, ha több olyan, egy vagy több folyamattal rendelkező adat-előállítót is létre kell hoznia, amelyek ugyanazt a logikát alkalmazzák, de az egyes adat-előállítók különböző Azure-tárfiókokat és Azure SQL Database-fiókokat használnak.</span><span class="sxs-lookup"><span data-stu-id="336bb-236">For example, you need to create many data factories with one or more pipelines that implement the same logic but each data factory uses different Azure storage and Azure SQL Database accounts.</span></span> <span data-ttu-id="336bb-237">Ebben a forgatókönyvben ugyanazt a sablont használja ugyanabban a környezetben (fejlesztői, teszt vagy éles) különböző paraméterfájlokkal a data factoryk létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="336bb-237">In this scenario, you use the same template in the same environment (dev, test, or production) with different parameter files to create data factories.</span></span> 

## <a name="resource-manager-template-for-creating-a-gateway"></a><span data-ttu-id="336bb-238">Resource Manager-sablon átjáró létrehozásához</span><span class="sxs-lookup"><span data-stu-id="336bb-238">Resource Manager template for creating a gateway</span></span>
<span data-ttu-id="336bb-239">Az itt látható, mintául szolgáló Resource Manager-sablonnal egy háttérben lévő logikai átjáró hozható létre.</span><span class="sxs-lookup"><span data-stu-id="336bb-239">Here is a sample Resource Manager template for creating a logical gateway in the back.</span></span> <span data-ttu-id="336bb-240">Telepítsen egy átjárót a helyszíni számítógépre vagy az Azure IaaS virtuális gépre, és regisztrálja az átjárót egy kulccsal a Data Factory szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="336bb-240">Install a gateway on your on-premises computer or Azure IaaS VM and register the gateway with Data Factory service using a key.</span></span> <span data-ttu-id="336bb-241">További információkért lásd: [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) (Adatok áthelyezése a helyszíni rendszer és a felhő között).</span><span class="sxs-lookup"><span data-stu-id="336bb-241">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for details.</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
    },
    "variables": {
        "dataFactoryName":  "GatewayUsingArmDF",
        "apiVersion": "2015-10-01",
        "singleQuote": "'"
    },
    "resources": [
        {
            "name": "[variables('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "eastus",
            "resources": [
                {
                    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                    "type": "gateways",
                    "apiVersion": "[variables('apiVersion')]",
                    "name": "GatewayUsingARM",
                    "properties": {
                        "description": "my gateway"
                    }
                }            
            ]
        }
    ]
}
```
<span data-ttu-id="336bb-242">Ez a sablon létrehozza a GatewayUsingArmDF nevű data factoryt a GatewayUsingARM nevű átjáróval.</span><span class="sxs-lookup"><span data-stu-id="336bb-242">This template creates a data factory named GatewayUsingArmDF with a gateway named: GatewayUsingARM.</span></span> 

## <a name="see-also"></a><span data-ttu-id="336bb-243">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="336bb-243">See Also</span></span>
| <span data-ttu-id="336bb-244">Témakör</span><span class="sxs-lookup"><span data-stu-id="336bb-244">Topic</span></span> | <span data-ttu-id="336bb-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="336bb-245">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="336bb-246">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="336bb-246">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="336bb-247">Ennek a cikknek a segítségével megismerheti a Azure Data Factory folyamatait és tevékenységeit, és megtudhatja, hogyan hozhat létre velük teljes körű, adatvezérelt munkafolyamatokat saját forgatókönyvéhez vagy vállalkozásához.</span><span class="sxs-lookup"><span data-stu-id="336bb-247">This article helps you understand pipelines and activities in Azure Data Factory and how to use them to construct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="336bb-248">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="336bb-248">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="336bb-249">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="336bb-249">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="336bb-250">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="336bb-250">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="336bb-251">Ez a cikk ismerteti az Azure Data Factory-alkalmazásmodell ütemezési és végrehajtási aspektusait.</span><span class="sxs-lookup"><span data-stu-id="336bb-251">This article explains the scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="336bb-252">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="336bb-252">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="336bb-253">Ez a cikk ismerteti, hogyan figyelheti és felügyelheti a folyamatokat, illetve hogyan kereshet bennük hibákat a Monitoring & Management App használatával.</span><span class="sxs-lookup"><span data-stu-id="336bb-253">This article describes how to monitor, manage, and debug pipelines using the Monitoring & Management App.</span></span> |

