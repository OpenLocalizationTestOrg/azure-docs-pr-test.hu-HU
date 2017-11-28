---
title: "Oktatóanyag: Folyamatok létrehozása a Resource Manager-sablon használatával | Microsoft Docs"
description: "Ebben az oktatóanyagban egy egyszerű Azure Data Factory-folyamatot fog létrehozni egy Azure Resource Manager-sablon segítségével. Ez az adatcsatorna adatok Azure blob storage tooan Azure SQL-adatbázis másolja át."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1274e11a-e004-4df5-af07-850b2de7c15e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 1c7567cb0423f7ce3e0cab2d77a4d861b70eb56b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a><span data-ttu-id="b3f16-104">Oktatóanyag: Használata Azure Resource Manager sablon toocreate egy adat-előállító adatcsatorna toocopy adatok</span><span class="sxs-lookup"><span data-stu-id="b3f16-104">Tutorial: Use Azure Resource Manager template toocreate a Data Factory pipeline toocopy data</span></span> 
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b3f16-105">Áttekintés és előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b3f16-105">Overview and prerequisites</span></span>](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [<span data-ttu-id="b3f16-106">Másolás varázsló</span><span class="sxs-lookup"><span data-stu-id="b3f16-106">Copy Wizard</span></span>](data-factory-copy-data-wizard-tutorial.md)
> * [<span data-ttu-id="b3f16-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b3f16-107">Azure portal</span></span>](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [<span data-ttu-id="b3f16-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3f16-108">Visual Studio</span></span>](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [<span data-ttu-id="b3f16-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="b3f16-109">PowerShell</span></span>](data-factory-copy-activity-tutorial-using-powershell.md)
> * [<span data-ttu-id="b3f16-110">Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="b3f16-110">Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [<span data-ttu-id="b3f16-111">REST API</span><span class="sxs-lookup"><span data-stu-id="b3f16-111">REST API</span></span>](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [<span data-ttu-id="b3f16-112">.NET API</span><span class="sxs-lookup"><span data-stu-id="b3f16-112">.NET API</span></span>](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

<span data-ttu-id="b3f16-113">Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate egy az Azure data factory.</span><span class="sxs-lookup"><span data-stu-id="b3f16-113">This tutorial shows you how toouse an Azure Resource Manager template toocreate an Azure data factory.</span></span> <span data-ttu-id="b3f16-114">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-114">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="b3f16-115">Nem alakítja át a bemeneti adatok tooproduce kimeneti adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-115">It does not transform input data tooproduce output data.</span></span> <span data-ttu-id="b3f16-116">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-116">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span>

<span data-ttu-id="b3f16-117">Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="b3f16-117">In this tutorial, you create a pipeline with one activity in it: Copy Activity.</span></span> <span data-ttu-id="b3f16-118">hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-118">hello copy activity copies data from a supported data store tooa supported sink data store.</span></span> <span data-ttu-id="b3f16-119">A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span><span class="sxs-lookup"><span data-stu-id="b3f16-119">For a list of data stores supported as sources and sinks, see [supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats).</span></span> <span data-ttu-id="b3f16-120">hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="b3f16-120">hello activity is powered by a globally available service that can copy data between various data stores in a secure, reliable, and scalable way.</span></span> <span data-ttu-id="b3f16-121">További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-121">For more information about hello Copy Activity, see [Data Movement Activities](data-factory-data-movement-activities.md).</span></span>

<span data-ttu-id="b3f16-122">Egy folyamathoz több tevékenység is tartozhat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-122">A pipeline can have more than one activity.</span></span> <span data-ttu-id="b3f16-123">És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását.</span><span class="sxs-lookup"><span data-stu-id="b3f16-123">And, you can chain two activities (run one activity after another) by setting hello output dataset of one activity as hello input dataset of hello other activity.</span></span> <span data-ttu-id="b3f16-124">További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span><span class="sxs-lookup"><span data-stu-id="b3f16-124">For more information, see [multiple activities in a pipeline](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).</span></span> 

> [!NOTE] 
> <span data-ttu-id="b3f16-125">hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-125">hello data pipeline in this tutorial copies data from a source data store tooa destination data store.</span></span> <span data-ttu-id="b3f16-126">Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-126">For a tutorial on how tootransform data using Azure Data Factory, see [Tutorial: Build a pipeline tootransform data using Hadoop cluster](data-factory-build-your-first-pipeline.md).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="b3f16-127">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b3f16-127">Prerequisites</span></span>
* <span data-ttu-id="b3f16-128">Lépkedjen végig [oktatóanyag – áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) és teljes hello **előfeltétel** lépéseket.</span><span class="sxs-lookup"><span data-stu-id="b3f16-128">Go through [Tutorial Overview and Prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) and complete hello **prerequisite** steps.</span></span>
* <span data-ttu-id="b3f16-129">Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b3f16-129">Follow instructions in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) article tooinstall latest version of Azure PowerShell on your computer.</span></span> <span data-ttu-id="b3f16-130">Ebben az oktatóanyagban a PowerShell toodeploy adat-előállító entitások használja.</span><span class="sxs-lookup"><span data-stu-id="b3f16-130">In this tutorial, you use PowerShell toodeploy Data Factory entities.</span></span> 
* <span data-ttu-id="b3f16-131">(választható) Lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="b3f16-131">(optional) See [Authoring Azure Resource Manager Templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn about Azure Resource Manager templates.</span></span>

## <a name="in-this-tutorial"></a><span data-ttu-id="b3f16-132">Az oktatóanyag tartalma</span><span class="sxs-lookup"><span data-stu-id="b3f16-132">In this tutorial</span></span>
<span data-ttu-id="b3f16-133">Ebben az oktatóanyagban a Data Factory entitások következő hello hoz létre egy adat-előállító:</span><span class="sxs-lookup"><span data-stu-id="b3f16-133">In this tutorial, you create a data factory with hello following Data Factory entities:</span></span>

| <span data-ttu-id="b3f16-134">Entitás</span><span class="sxs-lookup"><span data-stu-id="b3f16-134">Entity</span></span> | <span data-ttu-id="b3f16-135">Leírás</span><span class="sxs-lookup"><span data-stu-id="b3f16-135">Description</span></span> |
| --- | --- |
| <span data-ttu-id="b3f16-136">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-136">Azure Storage linked service</span></span> |<span data-ttu-id="b3f16-137">Az Azure Storage-fiók toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b3f16-137">Links your Azure Storage account toohello data factory.</span></span> <span data-ttu-id="b3f16-138">Az Azure Storage hello forrás adattár pedig Azure SQL adatbázis hello fogadó adattár hello másolási tevékenységhez hello oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="b3f16-138">Azure Storage is hello source data store and Azure SQL database is hello sink data store for hello copy activity in hello tutorial.</span></span> <span data-ttu-id="b3f16-139">Azt adja meg a hello másolási tevékenységhez hello a bemeneti adatokat tartalmazó hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b3f16-139">It specifies hello storage account that contains hello input data for hello copy activity.</span></span> |
| <span data-ttu-id="b3f16-140">Azure SQL Database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-140">Azure SQL Database linked service</span></span> |<span data-ttu-id="b3f16-141">Az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b3f16-141">Links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="b3f16-142">Hello másolási tevékenységhez hello kimeneti adatokat tartalmazó hello Azure SQL-adatbázist határoz meg.</span><span class="sxs-lookup"><span data-stu-id="b3f16-142">It specifies hello Azure SQL database that holds hello output data for hello copy activity.</span></span> |
| <span data-ttu-id="b3f16-143">Azure Blob bemeneti adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b3f16-143">Azure Blob input dataset</span></span> |<span data-ttu-id="b3f16-144">Toohello Azure tárolás társított szolgáltatásának hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b3f16-144">Refers toohello Azure Storage linked service.</span></span> <span data-ttu-id="b3f16-145">hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello bemeneti adatokat tartalmazó hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="b3f16-145">hello linked service refers tooan Azure Storage account and hello Azure Blob dataset specifies hello container, folder, and file name in hello storage that holds hello input data.</span></span> |
| <span data-ttu-id="b3f16-146">Az Azure SQL kimeneti adatkészlete</span><span class="sxs-lookup"><span data-stu-id="b3f16-146">Azure SQL output dataset</span></span> |<span data-ttu-id="b3f16-147">Toohello Azure SQL társított szolgáltatás hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="b3f16-147">Refers toohello Azure SQL linked service.</span></span> <span data-ttu-id="b3f16-148">hello Azure SQL társított szolgáltatást hivatkozik tooan Azure SQL-kiszolgálót, és hello Azure SQL dataset hello kimeneti adatokat tartalmazó táblát hello hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="b3f16-148">hello Azure SQL linked service refers tooan Azure SQL server and hello Azure SQL dataset specifies hello name of hello table that holds hello output data.</span></span> |
| <span data-ttu-id="b3f16-149">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="b3f16-149">Data pipeline</span></span> |<span data-ttu-id="b3f16-150">hello folyamat rendelkezik egy tevékenységet, írja be, amely az Azure blob-adathalmazra hello bemenetként másolása és hello kimenetként Azure SQL-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="b3f16-150">hello pipeline has one activity of type Copy that takes hello Azure blob dataset as an input and hello Azure SQL dataset as an output.</span></span> <span data-ttu-id="b3f16-151">hello másolási tevékenység hello Azure SQL Database az Azure blob tooa táblából másolja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-151">hello copy activity copies data from an Azure blob tooa table in hello Azure SQL database.</span></span> |

<span data-ttu-id="b3f16-152">A data factory egy vagy több folyamattal rendelkezhet.</span><span class="sxs-lookup"><span data-stu-id="b3f16-152">A data factory can have one or more pipelines.</span></span> <span data-ttu-id="b3f16-153">A folyamaton belül egy vagy több tevékenység lehet.</span><span class="sxs-lookup"><span data-stu-id="b3f16-153">A pipeline can have one or more activities in it.</span></span> <span data-ttu-id="b3f16-154">Kétféle típusú tevékenység létezik: az [adattovábbítási tevékenységek](data-factory-data-movement-activities.md) és az [adatátalakítási tevékenységek](data-factory-data-transformation-activities.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-154">There are two types of activities: [data movement activities](data-factory-data-movement-activities.md) and [data transformation activities](data-factory-data-transformation-activities.md).</span></span> <span data-ttu-id="b3f16-155">Az oktatóanyag során létrehoz egy egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot.</span><span class="sxs-lookup"><span data-stu-id="b3f16-155">In this tutorial, you create a pipeline with one activity (copy activity).</span></span>

![Azure Blob tooAzure SQL-adatbázis másolása](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

<span data-ttu-id="b3f16-157">hello következő ismertetett hello teljes Resource Manager-sablon adat-előállító entitások meghatározása, hogy gyorsan hello oktatóanyag és tesztelési hello sablon segítségével futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b3f16-157">hello following section provides hello complete Resource Manager template for defining Data Factory entities so that you can quickly run through hello tutorial and test hello template.</span></span> <span data-ttu-id="b3f16-158">toounderstand hogyan minden adat-előállító entitáshoz van definiálva, lásd: [adat-előállító entitások hello sablonban](#data-factory-entities-in-the-template) szakasz.</span><span class="sxs-lookup"><span data-stu-id="b3f16-158">toounderstand how each Data Factory entity is defined, see [Data Factory entities in hello template](#data-factory-entities-in-the-template) section.</span></span>

## <a name="data-factory-json-template"></a><span data-ttu-id="b3f16-159">Data Factory JSON-sablon</span><span class="sxs-lookup"><span data-stu-id="b3f16-159">Data Factory JSON template</span></span>
<span data-ttu-id="b3f16-160">hello legfelső szintű erőforrás-kezelő sablon meghatározásához egy adat-előállítót a következő:</span><span class="sxs-lookup"><span data-stu-id="b3f16-160">hello top-level Resource Manager template for defining a data factory is:</span></span> 

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
<span data-ttu-id="b3f16-161">Hozzon létre egy JSON fájlt **ADFCopyTutorialARM.json** a **C:\ADFGetStarted** hello tartalom a következő mappában:</span><span class="sxs-lookup"><span data-stu-id="b3f16-161">Create a JSON file named **ADFCopyTutorialARM.json** in **C:\ADFGetStarted** folder with hello following content:</span></span>

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of hello Azure storage account that contains hello data toobe copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for hello Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of hello blob container in hello Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of hello blob in hello container that has hello data toobe copied tooAzure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Server that will hold hello output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of hello Azure SQL Database in hello Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of hello user that has access toohello Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for hello user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in hello Azure SQL Database that will hold hello copied data." } 
      } 
    },
    "variables": {
      "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
      "azureSqlLinkedServiceName": "AzureSqlLinkedService",
      "azureStorageLinkedServiceName": "AzureStorageLinkedService",
      "blobInputDatasetName": "BlobInputDataset",
      "sqlOutputDatasetName": "SQLOutputDataset",
      "pipelineName": "Blob2SQLPipeline"
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
            "name": "[variables('azureSqlLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlDatabase",
              "description": "Azure SQL linked service",
              "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
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
              "structure": [
                {
                  "name": "Column0",
                  "type": "String"
                },
                {
                  "name": "Column1",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
                }
              },
              "availability": {
                "frequency": "Hour",
                "interval": 1
              },
              "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('sqlOutputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureSqlLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlTable",
              "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
              "structure": [
                {
                  "name": "FirstName",
                  "type": "String"
                },
                {
                  "name": "LastName",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
              },
              "availability": {
                "frequency": "Hour",
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
              "[variables('azureSqlLinkedServiceName')]",
              "[variables('blobInputDatasetName')]",
              "[variables('sqlOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "activities": [
                {
                  "name": "CopyFromAzureBlobToAzureSQL",
                  "description": "Copy data frm Azure blob tooAzure SQL",
                  "type": "Copy",
                  "inputs": [
                    {
                      "name": "[variables('blobInputDatasetName')]"
                    }
                  ],
                  "outputs": [
                    {
                      "name": "[variables('sqlOutputDatasetName')]"
                    }
                  ],
                  "typeProperties": {
                    "source": {
                      "type": "BlobSource"
                    },
                    "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                  },
                  "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                  }
                }
              ],
              "start": "2017-05-11T00:00:00Z",
              "end": "2017-05-12T00:00:00Z"
            }
          }
        ]
      }
    ]
  }
```

## <a name="parameters-json"></a><span data-ttu-id="b3f16-162">Paramétereket tartalmazó JSON-file</span><span class="sxs-lookup"><span data-stu-id="b3f16-162">Parameters JSON</span></span>
<span data-ttu-id="b3f16-163">Hozzon létre egy JSON fájlt **ADFCopyTutorialARM-Parameters.json** tartalmazó hello Azure Resource Manager sablon paramétereit.</span><span class="sxs-lookup"><span data-stu-id="b3f16-163">Create a JSON file named **ADFCopyTutorialARM-Parameters.json** that contains parameters for hello Azure Resource Manager template.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="b3f16-164">Adja meg az Azure Storage-fiók nevét és kulcsát a storageAccountName és a storageAccountKey paraméterek értékeiként.</span><span class="sxs-lookup"><span data-stu-id="b3f16-164">Specify name and key of your Azure Storage account for storageAccountName and storageAccountKey parameters.</span></span>  
> 
> <span data-ttu-id="b3f16-165">Adja meg az Azure-beli SQL-kiszolgálót, az adatbázist, a felhasználót és a jelszót az sqlServerName, a databaseName, az sqlServerUserName és az sqlServerPassword paraméterek értékeiként.</span><span class="sxs-lookup"><span data-stu-id="b3f16-165">Specify Azure SQL server, database, user, and password for sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters.</span></span>  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of hello Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for hello Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of hello Azure SQL server>" },
        "databaseName": { "value": "<Name of hello Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of hello user who has access toohello Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for hello user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> <span data-ttu-id="b3f16-166">Előfordulhat, hogy külön paraméter JSON-fájlok fejlesztési, tesztelési, és csak akkor használhatja az üzemi környezetek hello azonos Data Factory JSON-sablon.</span><span class="sxs-lookup"><span data-stu-id="b3f16-166">You may have separate parameter JSON files for development, testing, and production environments that you can use with hello same Data Factory JSON template.</span></span> <span data-ttu-id="b3f16-167">Ezekben a környezetekben automatizálhatja a Data Factory-entitások üzembe helyezését egy PowerShell-szkript használatával.</span><span class="sxs-lookup"><span data-stu-id="b3f16-167">By using a Power Shell script, you can automate deploying Data Factory entities in these environments.</span></span>  
> 
> 

## <a name="create-data-factory"></a><span data-ttu-id="b3f16-168">Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3f16-168">Create data factory</span></span>
1. <span data-ttu-id="b3f16-169">Start **Azure PowerShell** és futtatási hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b3f16-169">Start **Azure PowerShell** and run hello following command:</span></span>
   * <span data-ttu-id="b3f16-170">Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="b3f16-170">Run hello following command and enter hello user name and password that you use toosign in toohello Azure portal.</span></span>
   
    ```PowerShell
    Login-AzureRmAccount    
    ```  
   * <span data-ttu-id="b3f16-171">Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="b3f16-171">Run hello following command tooview all hello subscriptions for this account.</span></span>
   
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * <span data-ttu-id="b3f16-172">Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.</span><span class="sxs-lookup"><span data-stu-id="b3f16-172">Run hello following command tooselect hello subscription that you want toowork with.</span></span>
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. <span data-ttu-id="b3f16-173">Futtassa a következő parancs toodeploy adat-előállító entitások 1. lépésben létrehozott hello Resource Manager-sablon használatával hello.</span><span class="sxs-lookup"><span data-stu-id="b3f16-173">Run hello following command toodeploy Data Factory entities using hello Resource Manager template you created in Step 1.</span></span>

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a><span data-ttu-id="b3f16-174">Folyamat figyelése</span><span class="sxs-lookup"><span data-stu-id="b3f16-174">Monitor pipeline</span></span>

1. <span data-ttu-id="b3f16-175">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="b3f16-175">Log in toohello [Azure portal](https://portal.azure.com) using your Azure account.</span></span>
2. <span data-ttu-id="b3f16-176">Kattintson a **adat-előállítók** hello bal oldali menüben (vagy) kattintson **további szolgáltatások** kattintson **adat-előállítók** alatt **ESZKÖZINTELLIGENCIA + ANALITIKA** kategória.</span><span class="sxs-lookup"><span data-stu-id="b3f16-176">Click **Data factories** on hello left menu (or) click **More services** and click **Data factories** under **INTELLIGENCE + ANALYTICS** category.</span></span>
   
    ![Adat-előállítók menü](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. <span data-ttu-id="b3f16-178">A hello **adat-előállítók** lapon keresse meg és a data factory (AzureBlobToAzureSQLDatabaseDF) található.</span><span class="sxs-lookup"><span data-stu-id="b3f16-178">In hello **Data factories** page, search for and find your data factory (AzureBlobToAzureSQLDatabaseDF).</span></span> 
   
    ![Adat-előállító keresése](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. <span data-ttu-id="b3f16-180">Kattintson az Azure Data Factoryre.</span><span class="sxs-lookup"><span data-stu-id="b3f16-180">Click your Azure data factory.</span></span> <span data-ttu-id="b3f16-181">Az adat-előállító hello hello otthoni oldal akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b3f16-181">You see hello home page for hello data factory.</span></span>
   
    ![Az adat-előállító kezdőlapja](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. <span data-ttu-id="b3f16-183">Kövesse az utasításokat [adatkészletek és a folyamat figyelése](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello feldolgozási sorban lévő és adatkészletek ebben az oktatóanyagban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b3f16-183">Follow instructions from [Monitor datasets and pipeline](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello pipeline and datasets you have created in this tutorial.</span></span> <span data-ttu-id="b3f16-184">A Visual Studio jelenleg nem támogatja a Data Factory-folyamatok monitorozását.</span><span class="sxs-lookup"><span data-stu-id="b3f16-184">Currently, Visual Studio does not support monitoring Data Factory pipelines.</span></span>
7. <span data-ttu-id="b3f16-185">A szelet esetén hello **készen** állapot, győződjön meg arról, hogy hello adatok másolt toohello **üres** hello Azure SQL adatbázis táblájában.</span><span class="sxs-lookup"><span data-stu-id="b3f16-185">When a slice is in hello **Ready** state, verify that hello data is copied toohello **emp** table in hello Azure SQL database.</span></span>


<span data-ttu-id="b3f16-186">Hogyan toouse az Azure portál paneleken toomonitor pipeline és adatkészletek létrehozott ebben az oktatóanyagban a további információkért lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) .</span><span class="sxs-lookup"><span data-stu-id="b3f16-186">For more information on how toouse Azure portal blades toomonitor pipeline and datasets you have created in this tutorial, see [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) .</span></span>

<span data-ttu-id="b3f16-187">Hogyan toouse hello figyelő & kezelése alkalmazás toomonitor az adatok folyamatok további információkért lásd: [figyelése és kezelése az Azure Data Factory folyamatok figyelése alkalmazással](data-factory-monitor-manage-app.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-187">For more information on how toouse hello Monitor & Manage application toomonitor your data pipelines, see [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md).</span></span>

## <a name="data-factory-entities-in-hello-template"></a><span data-ttu-id="b3f16-188">Data Factory entitások hello sablonban</span><span class="sxs-lookup"><span data-stu-id="b3f16-188">Data Factory entities in hello template</span></span>
### <a name="define-data-factory"></a><span data-ttu-id="b3f16-189">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="b3f16-189">Define data factory</span></span>
<span data-ttu-id="b3f16-190">Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:</span><span class="sxs-lookup"><span data-stu-id="b3f16-190">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

<span data-ttu-id="b3f16-191">hello dataFactoryName típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="b3f16-191">hello dataFactoryName is defined as:</span></span> 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

<span data-ttu-id="b3f16-192">Egy egyedi karakterlánc alapján hello erőforrás azonosítót.</span><span class="sxs-lookup"><span data-stu-id="b3f16-192">It is a unique string based on hello resource group ID.</span></span>  

### <a name="defining-data-factory-entities"></a><span data-ttu-id="b3f16-193">Data Factory-entitások definiálása</span><span class="sxs-lookup"><span data-stu-id="b3f16-193">Defining Data Factory entities</span></span>
<span data-ttu-id="b3f16-194">hello következő adat-előállító entitások definiált hello JSON-sablon:</span><span class="sxs-lookup"><span data-stu-id="b3f16-194">hello following Data Factory entities are defined in hello JSON template:</span></span> 

1. [<span data-ttu-id="b3f16-195">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-195">Azure Storage linked service</span></span>](#azure-storage-linked-service)
2. [<span data-ttu-id="b3f16-196">Azure SQL Database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-196">Azure SQL linked service</span></span>](#azure-sql-database-linked-service)
3. [<span data-ttu-id="b3f16-197">Azure Blob-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b3f16-197">Azure blob dataset</span></span>](#azure-blob-dataset)
4. [<span data-ttu-id="b3f16-198">Azure SQL-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b3f16-198">Azure SQL dataset</span></span>](#azure-sql-dataset)
5. [<span data-ttu-id="b3f16-199">Másolási tevékenységgel rendelkező adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="b3f16-199">Data pipeline with a copy activity</span></span>](#data-pipeline)

#### <a name="azure-storage-linked-service"></a><span data-ttu-id="b3f16-200">Azure Storage társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-200">Azure Storage linked service</span></span>
<span data-ttu-id="b3f16-201">hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban.</span><span class="sxs-lookup"><span data-stu-id="b3f16-201">hello AzureStorageLinkedService links your Azure storage account toohello data factory.</span></span> <span data-ttu-id="b3f16-202">Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-202">You created a container and uploaded data toothis storage account as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="b3f16-203">Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát.</span><span class="sxs-lookup"><span data-stu-id="b3f16-203">You specify hello name and key of your Azure storage account in this section.</span></span> <span data-ttu-id="b3f16-204">Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="b3f16-204">See [Azure Storage linked service](data-factory-azure-blob-connector.md#azure-storage-linked-service) for details about JSON properties used toodefine an Azure Storage linked service.</span></span> 

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

<span data-ttu-id="b3f16-205">hello connectionString hello storageAccountName és storageAccountKey paramétereket használja.</span><span class="sxs-lookup"><span data-stu-id="b3f16-205">hello connectionString uses hello storageAccountName and storageAccountKey parameters.</span></span> <span data-ttu-id="b3f16-206">Ezek a paraméterek az átadott konfigurációs fájl használatával hello értékeit.</span><span class="sxs-lookup"><span data-stu-id="b3f16-206">hello values for these parameters passed by using a configuration file.</span></span> <span data-ttu-id="b3f16-207">hello definition is változókat használ: hello sablonban meghatározott azureStroageLinkedService és dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="b3f16-207">hello definition also uses variables: azureStroageLinkedService and dataFactoryName defined in hello template.</span></span> 

#### <a name="azure-sql-database-linked-service"></a><span data-ttu-id="b3f16-208">Azure SQL Database társított szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b3f16-208">Azure SQL Database linked service</span></span>
<span data-ttu-id="b3f16-209">AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b3f16-209">AzureSqlLinkedService links your Azure SQL database toohello data factory.</span></span> <span data-ttu-id="b3f16-210">hello blobtárolóból másolt hello adatok az adatbázis tárolja.</span><span class="sxs-lookup"><span data-stu-id="b3f16-210">hello data that is copied from hello blob storage is stored in this database.</span></span> <span data-ttu-id="b3f16-211">Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span><span class="sxs-lookup"><span data-stu-id="b3f16-211">You created hello emp table in this database as part of [prerequisites](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).</span></span> <span data-ttu-id="b3f16-212">Ebben a szakaszban megadhatja hello Azure SQL-kiszolgáló neve, az adatbázis nevét, a felhasználónév és a felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b3f16-212">You specify hello Azure SQL server name, database name, user name, and user password in this section.</span></span> <span data-ttu-id="b3f16-213">Lásd: [Azure SQL társított szolgáltatásnak](data-factory-azure-sql-connector.md#linked-service-properties) JSON használt tulajdonságok toodefine vonatkozó további információért az Azure SQL társított szolgáltatásnak.</span><span class="sxs-lookup"><span data-stu-id="b3f16-213">See [Azure SQL linked service](data-factory-azure-sql-connector.md#linked-service-properties) for details about JSON properties used toodefine an Azure SQL linked service.</span></span>  

```json
{
    "type": "linkedservices",
    "name": "[variables('azureSqlLinkedServiceName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlDatabase",
          "description": "Azure SQL linked service",
          "typeProperties": {
            "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
          }
    }
}
```

<span data-ttu-id="b3f16-214">hello connectionString használ sqlServerName, databaseName, sqlServerUserName és sqlServerPassword paraméterek, amelynek érték lett átadva a konfigurációs fájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="b3f16-214">hello connectionString uses sqlServerName, databaseName, sqlServerUserName, and sqlServerPassword parameters whose values are passed by using a configuration file.</span></span> <span data-ttu-id="b3f16-215">hello definition is használja következő változók hello sablonból hello: azureSqlLinkedServiceName, dataFactoryName.</span><span class="sxs-lookup"><span data-stu-id="b3f16-215">hello definition also uses hello following variables from hello template: azureSqlLinkedServiceName, dataFactoryName.</span></span>

#### <a name="azure-blob-dataset"></a><span data-ttu-id="b3f16-216">Azure Blob-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b3f16-216">Azure blob dataset</span></span>
<span data-ttu-id="b3f16-217">hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="b3f16-217">hello Azure storage linked service specifies hello connection string that Data Factory service uses at run time tooconnect tooyour Azure storage account.</span></span> <span data-ttu-id="b3f16-218">Az Azure blob adatkészlet-definícióban adja meg a blob-tároló, mappa, illetve hello bemeneti adatokat tartalmazó fájlt.</span><span class="sxs-lookup"><span data-stu-id="b3f16-218">In Azure blob dataset definition, you specify names of blob container, folder, and file that contains hello input data.</span></span> <span data-ttu-id="b3f16-219">Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="b3f16-219">See [Azure Blob dataset properties](data-factory-azure-blob-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure Blob dataset.</span></span> 

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
        "structure": [
        {
              "name": "Column0",
              "type": "String"
        },
        {
              "name": "Column1",
              "type": "String"
        }
          ],
          "typeProperties": {
            "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
            "fileName": "[parameters('sourceBlobName')]",
            "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
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

#### <a name="azure-sql-dataset"></a><span data-ttu-id="b3f16-220">Azure SQL-adatkészlet</span><span class="sxs-lookup"><span data-stu-id="b3f16-220">Azure SQL dataset</span></span>
<span data-ttu-id="b3f16-221">Hello Azure SQL-adatbázis, amely a másolt hello adatait hello Azure Blob Storage tárolóban tárolja hello tábla hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="b3f16-221">You specify hello name of hello table in hello Azure SQL database that holds hello copied data from hello Azure Blob storage.</span></span> <span data-ttu-id="b3f16-222">Lásd: [Azure SQL adatkészlet tulajdonságai](data-factory-azure-sql-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure SQL-adatkészlet vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="b3f16-222">See [Azure SQL dataset properties](data-factory-azure-sql-connector.md#dataset-properties) for details about JSON properties used toodefine an Azure SQL dataset.</span></span> 

```json
{
    "type": "datasets",
    "name": "[variables('sqlOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureSqlLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlTable",
          "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
          "structure": [
        {
              "name": "FirstName",
              "type": "String"
        },
        {
              "name": "LastName",
              "type": "String"
        }
          ],
          "typeProperties": {
            "tableName": "[parameters('targetSQLTable')]"
          },
          "availability": {
            "frequency": "Hour",
            "interval": 1
          }
    }
}
```

#### <a name="data-pipeline"></a><span data-ttu-id="b3f16-223">Adatfolyamat</span><span class="sxs-lookup"><span data-stu-id="b3f16-223">Data pipeline</span></span>
<span data-ttu-id="b3f16-224">Megadhatja egy folyamatot, amely adatokat másol hello Azure blob adatkészlet toohello Azure SQL-adatkészlet.</span><span class="sxs-lookup"><span data-stu-id="b3f16-224">You define a pipeline that copies data from hello Azure blob dataset toohello Azure SQL dataset.</span></span> <span data-ttu-id="b3f16-225">Lásd: [adatcsatorna JSON](data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását.</span><span class="sxs-lookup"><span data-stu-id="b3f16-225">See [Pipeline JSON](data-factory-create-pipelines.md#pipeline-json) for descriptions of JSON elements used toodefine a pipeline in this example.</span></span> 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('azureSqlLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "activities": [
        {
              "name": "CopyFromAzureBlobToAzureSQL",
              "description": "Copy data frm Azure blob tooAzure SQL",
              "type": "Copy",
              "inputs": [
            {
                  "name": "[variables('blobInputDatasetName')]"
            }
              ],
              "outputs": [
            {
                  "name": "[variables('sqlOutputDatasetName')]"
            }
              ],
              "typeProperties": {
                "source": {
                      "type": "BlobSource"
                },
                "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                }
              },
              "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
              }
        }
          ],
          "start": "2017-05-11T00:00:00Z",
          "end": "2017-05-12T00:00:00Z"
    }
}
```

## <a name="reuse-hello-template"></a><span data-ttu-id="b3f16-226">Újbóli hello sablon</span><span class="sxs-lookup"><span data-stu-id="b3f16-226">Reuse hello template</span></span>
<span data-ttu-id="b3f16-227">Hello oktatóanyagban létrehozott egy sablont a Data Factory entitásokat és a paraméterek értékeit, hogy a sablon meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="b3f16-227">In hello tutorial, you created a template for defining Data Factory entities and a template for passing values for parameters.</span></span> <span data-ttu-id="b3f16-228">hello csővezeték adatok Azure Storage fiók tooan Azure SQL-adatbázis paraméterek keresztül másolja át.</span><span class="sxs-lookup"><span data-stu-id="b3f16-228">hello pipeline copies data from an Azure Storage account tooan Azure SQL database specified via parameters.</span></span> <span data-ttu-id="b3f16-229">toouse hello egyező sablon toodeploy adat-előállító entitások toodifferent környezetekhez, hozzon létre egy paraméter környezetben, és használatra, ha toothat környezet telepítése.</span><span class="sxs-lookup"><span data-stu-id="b3f16-229">toouse hello same template toodeploy Data Factory entities toodifferent environments, you create a parameter file for each environment and use it when deploying toothat environment.</span></span>     

<span data-ttu-id="b3f16-230">Példa:</span><span class="sxs-lookup"><span data-stu-id="b3f16-230">Example:</span></span>  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

<span data-ttu-id="b3f16-231">Figyelje meg, hogy az első parancs által használt paraméter hello hello fejlesztői környezetben hello tesztkörnyezetben, a másikat a fájlt, és hello harmadik egy hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b3f16-231">Notice that hello first command uses parameter file for hello development environment, second one for hello test environment, and hello third one for hello production environment.</span></span>  

<span data-ttu-id="b3f16-232">Hello sablon tooperform is felhasználhatja ismétlődő feladatokat.</span><span class="sxs-lookup"><span data-stu-id="b3f16-232">You can also reuse hello template tooperform repeated tasks.</span></span> <span data-ttu-id="b3f16-233">Például egy sok adat-előállítók kell toocreate, vagy további folyamatok, amelyek megvalósítják az azonos hello logikát, de minden adat-előállító külön tárolási és SQL-adatbázis fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="b3f16-233">For example, you need toocreate many data factories with one or more pipelines that implement hello same logic but each data factory uses different Storage and SQL Database accounts.</span></span> <span data-ttu-id="b3f16-234">Ebben a forgatókönyvben hello használhatja ugyanazt a sablont a hello ugyanabban a környezetben (fejlesztői, tesztelési vagy éles) különböző paraméterrel toocreate adat-előállítók fájlok.</span><span class="sxs-lookup"><span data-stu-id="b3f16-234">In this scenario, you use hello same template in hello same environment (dev, test, or production) with different parameter files toocreate data factories.</span></span>   

## <a name="next-steps"></a><span data-ttu-id="b3f16-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3f16-235">Next steps</span></span>
<span data-ttu-id="b3f16-236">Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt.</span><span class="sxs-lookup"><span data-stu-id="b3f16-236">In this tutorial, you used Azure blob storage as a source data store and an Azure SQL database as a destination data store in a copy operation.</span></span> <span data-ttu-id="b3f16-237">hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz:</span><span class="sxs-lookup"><span data-stu-id="b3f16-237">hello following table provides a list of data stores supported as sources and destinations by hello copy activity:</span></span> 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

<span data-ttu-id="b3f16-238">Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.</span><span class="sxs-lookup"><span data-stu-id="b3f16-238">toolearn about how toocopy data to/from a data store, click hello link for hello data store in hello table.</span></span>
