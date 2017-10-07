---
title: "aaaBuild az első adat-előállítóban (Resource Manager-sablon) |} Microsoft Docs"
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
ms.openlocfilehash: fa08cd1ac3a0e5c5bf4bd4c6bd9dfa6dba9f4319
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a><span data-ttu-id="7bae6-103">Oktatóanyag: Az első Azure data factory létrehozása Azure Resource Manager-sablon használatával</span><span class="sxs-lookup"><span data-stu-id="7bae6-103">Tutorial: Build your first Azure data factory using Azure Resource Manager template</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7bae6-104">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7bae6-104">Overview and prerequisites</span></span>](data-factory-build-your-first-pipeline.md)
> * [<span data-ttu-id="7bae6-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7bae6-105">Azure portal</span></span>](data-factory-build-your-first-pipeline-using-editor.md)
> * [<span data-ttu-id="7bae6-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7bae6-106">Visual Studio</span></span>](data-factory-build-your-first-pipeline-using-vs.md)
> * [<span data-ttu-id="7bae6-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7bae6-107">PowerShell</span></span>](data-factory-build-your-first-pipeline-using-powershell.md)
> * [<span data-ttu-id="7bae6-108">Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="7bae6-108">Resource Manager Template</span></span>](data-factory-build-your-first-pipeline-using-arm.md)
> * [<span data-ttu-id="7bae6-109">REST API</span><span class="sxs-lookup"><span data-stu-id="7bae6-109">REST API</span></span>](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

<span data-ttu-id="7bae6-110">Ebben a cikkben használhatja az Azure Resource Manager sablon toocreate az első az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="7bae6-110">In this article, you use an Azure Resource Manager template toocreate your first Azure data factory.</span></span> <span data-ttu-id="7bae6-111">toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.</span><span class="sxs-lookup"><span data-stu-id="7bae6-111">toodo hello tutorial using other tools/SDKs, select one of hello options from hello drop-down list.</span></span>

<span data-ttu-id="7bae6-112">Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**.</span><span class="sxs-lookup"><span data-stu-id="7bae6-112">hello pipeline in this tutorial has one activity: **HDInsight Hive activity**.</span></span> <span data-ttu-id="7bae6-113">Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="7bae6-113">This activity runs a hive script on an Azure HDInsight cluster that transforms input data tooproduce output data.</span></span> <span data-ttu-id="7bae6-114">hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.</span><span class="sxs-lookup"><span data-stu-id="7bae6-114">hello pipeline is scheduled toorun once a month between hello specified start and end times.</span></span> 

> [!NOTE]
> <span data-ttu-id="7bae6-115">hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-115">hello data pipeline in this tutorial transforms input data tooproduce output data.</span></span> <span data-ttu-id="7bae6-116">Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="7bae6-116">For a tutorial on how toocopy data using Azure Data Factory, see [Tutorial: Copy data from Blob Storage tooSQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span>
> 
> <span data-ttu-id="7bae6-117">Ebben az oktatóanyagban hello csővezeték típusú csak egy tevékenység rendelkezik: HDInsightHive.</span><span class="sxs-lookup"><span data-stu-id="7bae6-117">hello pipeline in this tutorial has only one activity of type: HDInsightHive.</span></span> <span data-ttu-id="7bae6-118">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-118">A pipeline can have more than one activity.</span></span> <span data-ttu-id="7bae6-119">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="7bae6-119">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="7bae6-120">További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="7bae6-120">For more information, see [scheduling and execution in Data Factory](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="7bae6-121">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7bae6-121">Prerequisites</span></span>
* <span data-ttu-id="7bae6-122">Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="7bae6-122">Read through [Tutorial Overview](data-factory-build-your-first-pipeline.md) article and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="7bae6-123">Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7bae6-123">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span>
* <span data-ttu-id="7bae6-124">Lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="7bae6-124">See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span> 

## <a name="in-this-tutorial"></a><span data-ttu-id="7bae6-125">Az oktatóanyag tartalma</span><span class="sxs-lookup"><span data-stu-id="7bae6-125">In this tutorial</span></span>
| <span data-ttu-id="7bae6-126">Entitás</span><span class="sxs-lookup"><span data-stu-id="7bae6-126">Entity</span></span> | <span data-ttu-id="7bae6-127">Leírás</span><span class="sxs-lookup"><span data-stu-id="7bae6-127">Description</span></span> |
| --- | --- |
| <span data-ttu-id="7bae6-128">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-128">Azure Storage linked service</span></span> |<span data-ttu-id="7bae6-129">Az Azure Storage-fiók toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7bae6-129">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="7bae6-130">hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="7bae6-130">hello Azure Storage account holds hello input and output data for hello pipeline in this sample.</span></span> |
| <span data-ttu-id="7bae6-131">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-131">HDInsight on-demand linked service</span></span> |<span data-ttu-id="7bae6-132">Egy igény szerinti HDInsight fürt toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7bae6-132">Links an on-demand HDInsight cluster toohello data factory.</span></span> <span data-ttu-id="7bae6-133">hello fürt automatikusan létrejön tooprocess adatokat, és hello feldolgozás befejezése után törlődik.</span><span class="sxs-lookup"><span data-stu-id="7bae6-133">hello cluster is automatically created for you tooprocess data and is deleted after hello processing is done.</span></span> |
| <span data-ttu-id="7bae6-134">Azure Blob bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-134">Azure Blob input dataset</span></span> |<span data-ttu-id="7bae6-135">Toohello Azure tárolás társított szolgáltatásának hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="7bae6-135">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="7bae6-136">hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello bemeneti adatokat tartalmazó hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-136">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="7bae6-137">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-137">Azure Blob output dataset</span></span> |<span data-ttu-id="7bae6-138">Toohello Azure tárolás társított szolgáltatásának hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="7bae6-138">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="7bae6-139">hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello kimeneti adatokat tartalmazó hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-139">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello output data.</span></span> |
| <span data-ttu-id="7bae6-140">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="7bae6-140">Data pipeline</span></span> |<span data-ttu-id="7bae6-141">hello folyamat rendelkezik egy tevékenység típusú HDInsightHive, amely hello bemeneti adatkészlet-t használ fel, és létrehozza hello kimeneti adatkészletet.</span><span class="sxs-lookup"><span data-stu-id="7bae6-141">hello pipeline has one activity of type HDInsightHive, which consumes hello input dataset and produces hello output dataset.</span></span> |

<span data-ttu-id="7bae6-142">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="7bae6-142">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="7bae6-143">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="7bae6-143">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="7bae6-144">Kétféle típusú tevékenység létezik: az [adattovábbítási tevékenységek](data-factory-data-movement-activities.md) és az [adatátalakítási tevékenységek](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="7bae6-144">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="7bae6-145">Az oktatóanyag segítségével egyetlen tevékenységgel (Hive-tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="7bae6-145">In this tutorial, you create a pipeline with one activity (Hive activity).</span></span>

<span data-ttu-id="7bae6-146">hello következő ismertetett hello teljes Resource Manager-sablon adat-előállító entitások meghatározása, hogy gyorsan hello oktatóanyag és tesztelési hello sablon segítségével futtathatja.</span><span class="sxs-lookup"><span data-stu-id="7bae6-146">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="7bae6-147">toounderstand hogyan minden adat-előállító entitáshoz van definiálva, lásd: [adat-előállító entitások hello sablonban](#data-factory-entities-in-the-template) szakasz.</span><span class="sxs-lookup"><span data-stu-id="7bae6-147">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="7bae6-148">Data Factory JSON-sablon</span><span class="sxs-lookup"><span data-stu-id="7bae6-148">Data Factory JSON template</span></span>
<span data-ttu-id="7bae6-149">hello legfelső szintű erőforrás-kezelő sablon meghatározásához egy adat-előállítót a következő:</span><span class="sxs-lookup"><span data-stu-id="7bae6-149">hello top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="7bae6-150">Hozzon létre egy JSON fájlt **ADFTutorialARM.json** a **C:\ADFGetStarted** hello tartalom a következő mappában:</span><span class="sxs-lookup"><span data-stu-id="7bae6-150">Create a JSON file named **ADFTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
        "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello input/output data." } },
          "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
          "blobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
          "inputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that has hello input file." } },
          "inputBlobName": { "type": "string", "metadata": { "description": "Name of hello input file/blob." } },
          "outputBlobFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that will hold hello transformed data." } },
          "hiveScriptFolder": { "type": "string", "metadata": { "description": "hello folder in hello blob container that contains hello Hive query file." } },
          "hiveScriptFile": { "type": "string", "metadata": { "description": "Name of hello hive query (HQL) file." } }
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
> <span data-ttu-id="7bae6-151">Az Azure data factory létrehozására szolgáló Resource Manager-sablonra a következő helyen találhat egy másik példát: [Oktatóanyag: Folyamat létrehozása másolási tevékenységgel és Azure Resource Manager-sablon használatával](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="7bae6-151">You can find another example of Resource Manager template for creating an Azure data factory on [Tutorial: Create a pipeline with Copy Activity using an Azure Resource Manager template](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).</span></span>  
> 
> 

## <a name="parameters-json"></a><span data-ttu-id="7bae6-152">Paramétereket tartalmazó JSON-file</span><span class="sxs-lookup"><span data-stu-id="7bae6-152">Parameters JSON</span></span>
<span data-ttu-id="7bae6-153">Hozzon létre egy JSON fájlt **ADFTutorialARM-Parameters.json** tartalmazó hello Azure Resource Manager sablon paramétereit.</span><span class="sxs-lookup"><span data-stu-id="7bae6-153">Create a JSON file named **ADFTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span>  

> [!IMPORTANT]
> <span data-ttu-id="7bae6-154">Adja meg a hello neve és kulcsa az Azure Storage-fiókjának hello **storageAccountName** és **storageAccountKey** paraméterek paraméter fájlban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-154">Specify hello name and key of your Azure Storage account for hello **storageAccountName** and **storageAccountKey** parameters in this parameter file.</span></span> 
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
> <span data-ttu-id="7bae6-155">Előfordulhat, hogy külön paraméter JSON-fájlok fejlesztési, tesztelési, és csak akkor használhatja az üzemi környezetek hello azonos Data Factory JSON-sablon.</span><span class="sxs-lookup"><span data-stu-id="7bae6-155">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="7bae6-156">Ezekben a környezetekben automatizálhatja a Data Factory-entitások üzembe helyezését egy PowerShell-szkript használatával.</span><span class="sxs-lookup"><span data-stu-id="7bae6-156">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span> 
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="7bae6-157">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="7bae6-157">Create data factory</span></span>
1. <span data-ttu-id="7bae6-158">Start **Azure PowerShell** és futtatási hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7bae6-158">Start **Azure PowerShell** and run hello following command:</span></span> 
   * <span data-ttu-id="7bae6-159">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7bae6-159">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * <span data-ttu-id="7bae6-160">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="7bae6-160">Run hello following command tooview all hello subscriptions for this account.</span></span>
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * <span data-ttu-id="7bae6-161">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="7bae6-161">Run hello following command tooselect hello subscription that you want toowork with.</span></span> <span data-ttu-id="7bae6-162">Ez az előfizetés ugyanaz, mint egy Azure-portálon hello használt hello kell hello.</span><span class="sxs-lookup"><span data-stu-id="7bae6-162">This subscription should be hello same as hello one you used in hello Azure portal.</span></span>
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. <span data-ttu-id="7bae6-163">Futtassa a következő parancs toodeploy adat-előállító entitások 1. lépésben létrehozott hello Resource Manager-sablon használatával hello.</span><span class="sxs-lookup"><span data-stu-id="7bae6-163">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span> 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="7bae6-164">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="7bae6-164">Monitor pipeline</span></span>
1. <span data-ttu-id="7bae6-165">Toohello a bejelentkezés után [Azure-portálon](https://portal.azure.com/), kattintson a **Tallózás** válassza **adat-előállítók**.</span><span class="sxs-lookup"><span data-stu-id="7bae6-165">After logging in toohello [Azure portal](https://portal.azure.com/), Click **Browse** and select **Data factories**.</span></span>
     <span data-ttu-id="7bae6-166">![Tallózás-&gt;Data Factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span><span class="sxs-lookup"><span data-stu-id="7bae6-166">![Browse->Data factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)</span></span>
2. <span data-ttu-id="7bae6-167">A hello **adat-előállítók** panelen kattintson az adat-előállító hello (**TutorialFactoryARM**) alapján létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7bae6-167">In hello **Data Factories** blade, click hello data factory (**TutorialFactoryARM**) you created.</span></span>    
3. <span data-ttu-id="7bae6-168">A hello **adat-előállító** a data factory paneljén kattintson **Diagram**.</span><span class="sxs-lookup"><span data-stu-id="7bae6-168">In hello **Data Factory** blade for your data factory, click **Diagram**.</span></span>

     ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. <span data-ttu-id="7bae6-170">A hello **diagramnézet**, az hello adatcsatornák áttekintés és adatkészletek szerepel ez az oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="7bae6-170">In hello **Diagram View**, you see an overview of hello pipelines, and datasets used in this tutorial.</span></span>
   
   ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. <span data-ttu-id="7bae6-172">A Diagram nézet hello, kattintson duplán a hello dataset **AzureBlobOutput**.</span><span class="sxs-lookup"><span data-stu-id="7bae6-172">In hello Diagram View, double-click hello dataset **AzureBlobOutput**.</span></span> <span data-ttu-id="7bae6-173">Láthatja, hogy hello szelet, amely folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="7bae6-173">You see that hello slice that is currently being processed.</span></span>
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. <span data-ttu-id="7bae6-175">Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát.</span><span class="sxs-lookup"><span data-stu-id="7bae6-175">When processing is done, you see hello slice in **Ready** state.</span></span> <span data-ttu-id="7bae6-176">Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig).</span><span class="sxs-lookup"><span data-stu-id="7bae6-176">Creation of an on-demand HDInsight cluster usually takes sometime (approximately 20 minutes).</span></span> <span data-ttu-id="7bae6-177">Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.</span><span class="sxs-lookup"><span data-stu-id="7bae6-177">Therefore, expect hello pipeline tootake **approximately 30 minutes** tooprocess hello slice.</span></span>
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. <span data-ttu-id="7bae6-179">Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-179">When hello slice is in **Ready** state, check hello **partitioneddata** folder in hello **adfgetstarted** container in your blob storage for hello output data.</span></span>  

<span data-ttu-id="7bae6-180">Lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) hogyan toouse hello Azure portál paneleken toomonitor hello feldolgozási sorban lévő és adatkészletek létrehozott ebben az oktatóanyagban kapcsolatos utasításokat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-180">See [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) for instructions on how toouse hello Azure portal blades toomonitor hello pipeline and datasets you have created in this tutorial.</span></span>

<span data-ttu-id="7bae6-181">Figyelő és App kezelése toomonitor az adatok folyamatok is használja.</span><span class="sxs-lookup"><span data-stu-id="7bae6-181">You can also use Monitor and Manage App toomonitor your data pipelines.</span></span> <span data-ttu-id="7bae6-182">Lásd: [figyelése és kezelése az Azure Data Factory folyamatok figyelése alkalmazással](data-factory-monitor-manage-app.md) hello alkalmazás használatával kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="7bae6-182">See [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) for details about using hello application.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="7bae6-183">hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="7bae6-183">hello input file gets deleted when hello slice is processed successfully.</span></span> <span data-ttu-id="7bae6-184">Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.</span><span class="sxs-lookup"><span data-stu-id="7bae6-184">Therefore, if you want toorerun hello slice or do hello tutorial again, upload hello input file (input.log) toohello inputdata folder of hello adfgetstarted container.</span></span>
> 
> 

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="7bae6-185">Data Factory entitások hello sablonban</span><span class="sxs-lookup"><span data-stu-id="7bae6-185">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="7bae6-186">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="7bae6-186">Define data factory</span></span>
<span data-ttu-id="7bae6-187">Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:</span><span class="sxs-lookup"><span data-stu-id="7bae6-187">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
<span data-ttu-id="7bae6-188">hello dataFactoryName típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="7bae6-188">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
<span data-ttu-id="7bae6-189">Egy egyedi karakterlánc alapján hello erőforrás azonosítót.</span><span class="sxs-lookup"><span data-stu-id="7bae6-189">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="7bae6-190">Data Factory-entitások definiálása</span><span class="sxs-lookup"><span data-stu-id="7bae6-190">Defining Data Factory entities</span></span>
<span data-ttu-id="7bae6-191">hello következő adat-előállító entitások definiált hello JSON-sablon:</span><span class="sxs-lookup"><span data-stu-id="7bae6-191">hello following Data Factory entities are defined in hello JSON template:</span></span> 

* [<span data-ttu-id="7bae6-192">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-192">Azure Storage linked service</span></span>](#azure-storage-linked-service)
* [<span data-ttu-id="7bae6-193">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-193">HDInsight on-demand linked service</span></span>](#hdinsight-on-demand-linked-service)
* [<span data-ttu-id="7bae6-194">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-194">Azure blob input dataset</span></span>](#azure-blob-input-dataset)
* [<span data-ttu-id="7bae6-195">Azure blobkimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-195">Azure blob output dataset</span></span>](#azure-blob-output-dataset)
* [<span data-ttu-id="7bae6-196">Másolási tevékenységgel rendelkező adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="7bae6-196">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="7bae6-197">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-197">Azure Storage linked service</span></span>
<span data-ttu-id="7bae6-198">Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="7bae6-198">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="7bae6-199">Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="7bae6-199">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

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
<span data-ttu-id="7bae6-200">Hello **connectionString** használ hello storageAccountName és storageAccountKey paraméterek.</span><span class="sxs-lookup"><span data-stu-id="7bae6-200">hello **connectionString** uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="7bae6-201">Ezek a paraméterek az átadott konfigurációs fájl használatával hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="7bae6-201">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="7bae6-202">hello definition is változókat használ: hello sablonban meghatározott azureStroageLinkedService és dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="7bae6-202">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="hdinsight-on-demand-linked-service"></a><span data-ttu-id="7bae6-203">HDInsight igény szerinti társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7bae6-203">HDInsight on-demand linked service</span></span>
<span data-ttu-id="7bae6-204">Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért cikk egy HDInsight igény társított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7bae6-204">See [Compute linked services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) article for details about JSON properties used toodefine an HDInsight on-demand linked service.</span></span>  

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
<span data-ttu-id="7bae6-205">Vegye figyelembe a következő pontok hello:</span><span class="sxs-lookup"><span data-stu-id="7bae6-205">Note hello following points:</span></span> 

* <span data-ttu-id="7bae6-206">hello adat-előállító létrehoz egy **Linux-alapú** a fenti JSON hello meg HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="7bae6-206">hello Data Factory creates a **Linux-based** HDInsight cluster for you with hello above JSON.</span></span> <span data-ttu-id="7bae6-207">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="7bae6-207">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span> 
* <span data-ttu-id="7bae6-208">Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-208">You could use **your own HDInsight cluster** instead of using an on-demand HDInsight cluster.</span></span> <span data-ttu-id="7bae6-209">További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="7bae6-209">See [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) for details.</span></span>
* <span data-ttu-id="7bae6-210">hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**).</span><span class="sxs-lookup"><span data-stu-id="7bae6-210">hello HDInsight cluster creates a **default container** in hello blob storage you specified in hello JSON (**linkedServiceName**).</span></span> <span data-ttu-id="7bae6-211">HDInsight nem törli a tárolót hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="7bae6-211">HDInsight does not delete this container when hello cluster is deleted.</span></span> <span data-ttu-id="7bae6-212">Ez a működésmód szándékos.</span><span class="sxs-lookup"><span data-stu-id="7bae6-212">This behavior is by design.</span></span> <span data-ttu-id="7bae6-213">Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz toobe (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.</span><span class="sxs-lookup"><span data-stu-id="7bae6-213">With on-demand HDInsight linked service, a HDInsight cluster is created every time a slice needs toobe processed unless there is an existing live cluster (**timeToLive**) and is deleted when hello processing is done.</span></span>
  
    <span data-ttu-id="7bae6-214">Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-214">As more slices are processed, you see many containers in your Azure blob storage.</span></span> <span data-ttu-id="7bae6-215">Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket.</span><span class="sxs-lookup"><span data-stu-id="7bae6-215">If you do not need them for troubleshooting of hello jobs, you may want toodelete them tooreduce hello storage cost.</span></span> <span data-ttu-id="7bae6-216">ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp".</span><span class="sxs-lookup"><span data-stu-id="7bae6-216">hello names of these containers follow a pattern: "adf**yourdatafactoryname**-**linkedservicename**-datetimestamp".</span></span> <span data-ttu-id="7bae6-217">Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.</span><span class="sxs-lookup"><span data-stu-id="7bae6-217">Use tools such as [Microsoft Storage Explorer](http://storageexplorer.com/) toodelete containers in your Azure blob storage.</span></span>

<span data-ttu-id="7bae6-218">További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="7bae6-218">See [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details.</span></span>

#### <a name="azure-blob-input-dataset"></a><span data-ttu-id="7bae6-219">Azure blobbemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-219">Azure blob input dataset</span></span>
<span data-ttu-id="7bae6-220">Megadhatja a blob-tároló, a mappa és a hello bemeneti adatokat tartalmazó fájlt hello nevét.</span><span class="sxs-lookup"><span data-stu-id="7bae6-220">You specify hello names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="7bae6-221">Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="7bae6-221">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

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
<span data-ttu-id="7bae6-222">Ez a definíció használja a következő paraméter sablonban definiált paraméterek hello: blobContainer, inputBlobFolder, és inputBlobName.</span><span class="sxs-lookup"><span data-stu-id="7bae6-222">This definition uses hello following parameters defined in parameter template: blobContainer, inputBlobFolder, and inputBlobName.</span></span> 

#### <a name="azure-blob-output-dataset"></a><span data-ttu-id="7bae6-223">Azure Blob kimeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="7bae6-223">Azure Blob output dataset</span></span>
<span data-ttu-id="7bae6-224">Megadhatja a hello blobtárolót és hello kimeneti adatokat tartalmazó mappa nevét.</span><span class="sxs-lookup"><span data-stu-id="7bae6-224">You specify hello names of blob container and folder that holds hello output data.</span></span> <span data-ttu-id="7bae6-225">Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="7bae6-225">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span>  

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

<span data-ttu-id="7bae6-226">Ez a definíció használja a következő paraméterek hello paraméter sablonban meghatározott hello: blobContainer és outputBlobFolder.</span><span class="sxs-lookup"><span data-stu-id="7bae6-226">This definition uses hello following parameters defined in hello parameter template: blobContainer and outputBlobFolder.</span></span> 

#### <a name="data-pipeline"></a><span data-ttu-id="7bae6-227">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="7bae6-227">Data pipeline</span></span>
<span data-ttu-id="7bae6-228">Definiálhat egy folyamatot, amely átalakítja az adatokat a Hive-parancsfájl egy igény szerinti Azure HDInsight-fürtön való futtatásával.</span><span class="sxs-lookup"><span data-stu-id="7bae6-228">You define a pipeline that transform data by running Hive script on an on-demand Azure HDInsight cluster.</span></span> <span data-ttu-id="7bae6-229">Lásd: [adatcsatorna JSON](data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását.</span><span class="sxs-lookup"><span data-stu-id="7bae6-229">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

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

## <a name="reuse-hello-template"></a><span data-ttu-id="7bae6-230">Újbóli hello sablon</span><span class="sxs-lookup"><span data-stu-id="7bae6-230">Reuse hello template</span></span>
<span data-ttu-id="7bae6-231">Hello oktatóanyagban létrehozott egy sablont a Data Factory entitásokat és a paraméterek értékeit, hogy a sablon meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="7bae6-231">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="7bae6-232">toouse hello egyező sablon toodeploy adat-előállító entitások toodifferent környezetekhez, hozzon létre egy paraméter környezetben, és használatra, ha toothat környezet telepítése.</span><span class="sxs-lookup"><span data-stu-id="7bae6-232">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="7bae6-233">Példa:</span><span class="sxs-lookup"><span data-stu-id="7bae6-233">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
<span data-ttu-id="7bae6-234">Figyelje meg, hogy az első parancs által használt paraméter hello hello fejlesztői környezetben hello tesztkörnyezetben, a másikat a fájlt, és hello harmadik egy hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7bae6-234">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="7bae6-235">Hello sablon tooperform is felhasználhatja ismétlődő feladatokat.</span><span class="sxs-lookup"><span data-stu-id="7bae6-235">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="7bae6-236">Például egy sok adat-előállítók kell toocreate, vagy további folyamatok, amelyek megvalósítják hello ugyanez a logika, de minden adatok gyári által használt különböző Azure storage és az Azure SQL Database-fiókok.</span><span class="sxs-lookup"><span data-stu-id="7bae6-236">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Azure storage and Azure SQL Database accounts.</span></span> <span data-ttu-id="7bae6-237">Ebben a forgatókönyvben hello használhatja ugyanazt a sablont a hello ugyanabban a környezetben (fejlesztői, tesztelési vagy éles) különböző paraméterrel toocreate adat-előállítók fájlok.</span><span class="sxs-lookup"><span data-stu-id="7bae6-237">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span> 

## <a name="resource-manager-template-for-creating-a-gateway"></a><span data-ttu-id="7bae6-238">Resource Manager-sablon átjáró létrehozásához</span><span class="sxs-lookup"><span data-stu-id="7bae6-238">Resource Manager template for creating a gateway</span></span>
<span data-ttu-id="7bae6-239">Íme egy minta logikai átjáró létrehozásához hello vissza a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="7bae6-239">Here is a sample Resource Manager template for creating a logical gateway in hello back.</span></span> <span data-ttu-id="7bae6-240">Átjáró telepítéséhez a helyi számítógépen vagy Azure IaaS virtuális Gépen, és hello átjáró regisztrálása kulccsal Data Factory szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-240">Install a gateway on your on-premises computer or Azure IaaS VM and register hello gateway with Data Factory service using a key.</span></span> <span data-ttu-id="7bae6-241">További információkért lásd: [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) (Adatok áthelyezése a helyszíni rendszer és a felhő között).</span><span class="sxs-lookup"><span data-stu-id="7bae6-241">See [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) for details.</span></span>

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
<span data-ttu-id="7bae6-242">Ez a sablon létrehozza a GatewayUsingArmDF nevű data factoryt a GatewayUsingARM nevű átjáróval.</span><span class="sxs-lookup"><span data-stu-id="7bae6-242">This template creates a data factory named GatewayUsingArmDF with a gateway named: GatewayUsingARM.</span></span> 

## <a name="see-also"></a><span data-ttu-id="7bae6-243">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7bae6-243">See Also</span></span>
| <span data-ttu-id="7bae6-244">Témakör</span><span class="sxs-lookup"><span data-stu-id="7bae6-244">Topic</span></span> | <span data-ttu-id="7bae6-245">Leírás</span><span class="sxs-lookup"><span data-stu-id="7bae6-245">Description</span></span> |
|:--- |:--- |
| [<span data-ttu-id="7bae6-246">Folyamatok</span><span class="sxs-lookup"><span data-stu-id="7bae6-246">Pipelines</span></span>](data-factory-create-pipelines.md) |<span data-ttu-id="7bae6-247">Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket.</span><span class="sxs-lookup"><span data-stu-id="7bae6-247">This article helps you understand pipelines and activities in Azure Data Factory and how toouse them tooconstruct end-to-end data-driven workflows for your scenario or business.</span></span> |
| [<span data-ttu-id="7bae6-248">Adatkészletek</span><span class="sxs-lookup"><span data-stu-id="7bae6-248">Datasets</span></span>](data-factory-create-datasets.md) |<span data-ttu-id="7bae6-249">Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban.</span><span class="sxs-lookup"><span data-stu-id="7bae6-249">This article helps you understand datasets in Azure Data Factory.</span></span> |
| [<span data-ttu-id="7bae6-250">Ütemezés és végrehajtás</span><span class="sxs-lookup"><span data-stu-id="7bae6-250">Scheduling and execution</span></span>](data-factory-scheduling-and-execution.md) |<span data-ttu-id="7bae6-251">Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait.</span><span class="sxs-lookup"><span data-stu-id="7bae6-251">This article explains hello scheduling and execution aspects of Azure Data Factory application model.</span></span> |
| [<span data-ttu-id="7bae6-252">Folyamatok figyelése és felügyelete a Monitoring App használatával</span><span class="sxs-lookup"><span data-stu-id="7bae6-252">Monitor and manage pipelines using Monitoring App</span></span>](data-factory-monitor-manage-app.md) |<span data-ttu-id="7bae6-253">Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7bae6-253">This article describes how toomonitor, manage, and debug pipelines using hello Monitoring & Management App.</span></span> |

