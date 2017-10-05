---
title: "Adat-előállítóban használata Resource Manager sablonok |} Microsoft Docs"
description: "Megtudhatja, hogyan történő létrehozásáról és használatáról az Azure Resource Manager-sablonok létrehozása a Data Factory entitásokat."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.assetid: 37724021-f55f-4e85-9206-6d4a48bda3d8
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: c3ea2c047434b5b5495f0ce85be9376a502e4962
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-templates-to-create-azure-data-factory-entities"></a><span data-ttu-id="2b153-103">Azure Data Factory entitásokat hozhatnak létre a sablonok segítségével</span><span class="sxs-lookup"><span data-stu-id="2b153-103">Use templates to create Azure Data Factory entities</span></span>
## <a name="overview"></a><span data-ttu-id="2b153-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="2b153-104">Overview</span></span>
<span data-ttu-id="2b153-105">Során azt tapasztalhatja, saját maga az adat-integráció igényeinek Azure Data Factory használatával újból felhasználja a ugyanilyen mintájú különböző környezetek vagy végrehajtási ugyanahhoz a megoldáshoz belül többször is ugyanezt a feladatot.</span><span class="sxs-lookup"><span data-stu-id="2b153-105">While using Azure Data Factory for your data integration needs, you may find yourself reusing the same pattern across different environments or implementing the same task repetitively within the same solution.</span></span> <span data-ttu-id="2b153-106">A sablonok révén megvalósítása és kezelése forgatókönyvekben egyszerű módon.</span><span class="sxs-lookup"><span data-stu-id="2b153-106">Templates help you implement and manage these scenarios in an easy manner.</span></span> <span data-ttu-id="2b153-107">Sablonok az Azure Data Factory ideálisak újrahasznosításának és ismétlődési forgatókönyveit.</span><span class="sxs-lookup"><span data-stu-id="2b153-107">Templates in Azure Data Factory are ideal for scenarios that involve reusability and repetition.</span></span>

<span data-ttu-id="2b153-108">Vegye figyelembe a helyzet, amikor egy szervezet rendelkezik a 10 gyártási tőzsdei árfolyamjelző keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="2b153-108">Consider the situation where an organization has 10 manufacturing plants across the world.</span></span> <span data-ttu-id="2b153-109">A naplók az egyes gépek önálló helyszíni SQL Server-adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="2b153-109">The logs from each plant are stored in a separate on-premises SQL Server database.</span></span> <span data-ttu-id="2b153-110">A vállalat szeretne a felhőben az alkalmi használatával egyetlen adatraktár létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2b153-110">The company wants to build a single data warehouse in the cloud for ad-hoc analytics.</span></span> <span data-ttu-id="2b153-111">Azt is szeretné, ha a ugyanez a logika, de a fejlesztői, tesztelési és éles környezetek különböző konfigurációkkal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="2b153-111">It also wants to have the same logic but different configurations for development, test, and production environments.</span></span>

<span data-ttu-id="2b153-112">Ebben az esetben egy feladatot meg kell ismételni belül ugyanabba a környezetbe, de eltérő értékű között az egyes gyár 10 adat-előállítók.</span><span class="sxs-lookup"><span data-stu-id="2b153-112">In this case, a task needs to be repeated within the same environment, but with different values across the 10 data factories for each manufacturing plant.</span></span> <span data-ttu-id="2b153-113">Gyakorlatilag **ismétlődési** található.</span><span class="sxs-lookup"><span data-stu-id="2b153-113">In effect, **repetition** is present.</span></span> <span data-ttu-id="2b153-114">Templating lehetővé teszi, hogy a elvonása, az általános folyamat (Ez azt jelenti, hogy ugyanazokat a tevékenységeket rendelkező minden adat-előállító adatcsatornák), de minden olyan üzem külön paraméterfájl használ.</span><span class="sxs-lookup"><span data-stu-id="2b153-114">Templating allows the abstraction of this generic flow (that is, pipelines having the same activities in each data factory), but uses a separate parameter file for each manufacturing plant.</span></span>

<span data-ttu-id="2b153-115">Továbbá, mivel a szervezet által telepítendő ezek 10 adat-előállítók többször különböző környezetek között, sablonok segítségével **újrahasznosításának** külön paraméterfájlt fejlesztésekhez felügyelniük teszteléséhez és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2b153-115">Furthermore, as the organization wants to deploy these 10 data factories multiple times across different environments, templates can use this **reusability** by utilizing separate parameter files for development, test, and production environments.</span></span>

## <a name="templating-with-azure-resource-manager"></a><span data-ttu-id="2b153-116">Az Azure Resource Manager eszközzel templating</span><span class="sxs-lookup"><span data-stu-id="2b153-116">Templating with Azure Resource Manager</span></span>
<span data-ttu-id="2b153-117">[Az Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md#template-deployment) nagyszerű módját az Azure Data Factory templating eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="2b153-117">[Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md#template-deployment) are a great way to achieve templating in Azure Data Factory.</span></span> <span data-ttu-id="2b153-118">Resource Manager-sablonok JSON-fájl határozza meg az infrastruktúra és az Azure-megoldás konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2b153-118">Resource Manager templates define the infrastructure and configuration of your Azure solution through a JSON file.</span></span> <span data-ttu-id="2b153-119">Mivel az Azure Resource Manager-sablonok működik a legtöbb vagy az összes Azure-szolgáltatásokkal, széles körben használat könnyedén kezelheti a Azure eszközök összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="2b153-119">Because Azure Resource Manager templates work with all/most Azure services, it can be widely used to easily manage all resources of your Azure assets.</span></span> <span data-ttu-id="2b153-120">Lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) további információt a Resource Manager-sablonok általában.</span><span class="sxs-lookup"><span data-stu-id="2b153-120">See [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) to learn more about the Resource Manager Templates in general.</span></span>

## <a name="tutorials"></a><span data-ttu-id="2b153-121">oktatóanyagokat</span><span class="sxs-lookup"><span data-stu-id="2b153-121">Tutorials</span></span>
<span data-ttu-id="2b153-122">Az alábbi oktatóanyagok adat-előállító entitások létrehozása a Resource Manager-sablonok használatával lépésenkénti útmutatót lásd:</span><span class="sxs-lookup"><span data-stu-id="2b153-122">See the following tutorials for step-by-step instructions to create Data Factory entities by using Resource Manager templates:</span></span>

* [<span data-ttu-id="2b153-123">Oktatóanyag: Adatok másolása az Azure Resource Manager-sablon használatával folyamat létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b153-123">Tutorial: Create a pipeline to copy data by using Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="2b153-124">Oktatóanyag: Hozzon létre egy folyamatot adatfeldolgozásra történő Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="2b153-124">Tutorial: Create a pipeline to process data by using Azure Resource Manager template</span></span>](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a><span data-ttu-id="2b153-125">Data Factory sablonok a Githubon</span><span class="sxs-lookup"><span data-stu-id="2b153-125">Data Factory templates on GitHub</span></span>
<span data-ttu-id="2b153-126">Tekintse meg a következő Azure gyors üzembe helyezési sablonokat a Githubon:</span><span class="sxs-lookup"><span data-stu-id="2b153-126">Check out the following Azure quick start templates on GitHub:</span></span>

* [<span data-ttu-id="2b153-127">Adatok másolása az Azure SQL Database az Azure Blob Storage tartalmazó Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b153-127">Create a Data factory to copy data from Azure Blob Storage to Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [<span data-ttu-id="2b153-128">Hozzon létre egy adat-előállító Hive tevékenység on Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="2b153-128">Create a Data factory with Hive activity on Azure HDInsight cluster</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [<span data-ttu-id="2b153-129">Adatok másolása Salesforce Azure-blobot tartalmazó Data factory létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b153-129">Create a Data factory to copy data from Salesforce to Azure Blobs</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [<span data-ttu-id="2b153-130">Hozzon létre egy adat-előállító tevékenységek láncában: adatokat másol egy FTP-kiszolgáló Azure BLOB, hívja meg az adatok átalakítására igény szerinti HDInsight-fürtök a hive parancsfájl, és másolja át az eredményt az Azure SQL-adatbázisba</span><span class="sxs-lookup"><span data-stu-id="2b153-130">Create a Data factory that chains activities: copies data from an FTP server to Azure Blobs, invokes a hive script on an on-demand HDInsight cluster to transform the data, and copies result into Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

<span data-ttu-id="2b153-131">Ahhoz, hogy az Azure Data Factory sablonokat nyugodtan [Azure gyors üzembe helyezési](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="2b153-131">Feel free to share your Azure Data Factory templates at [Azure Quick start](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="2b153-132">Tekintse meg a [hozzájárulás útmutató](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) sablonok, amelyek megoszthatók az ebben a tárházban keresztül fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="2b153-132">Refer to the [contribution guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) while developing templates that can be shared via this repository.</span></span>

<span data-ttu-id="2b153-133">A következő szakaszok részletesen bemutatják a Resource Manager-sablon adat-előállító erőforrások meghatározása.</span><span class="sxs-lookup"><span data-stu-id="2b153-133">The following sections provide details about defining Data Factory resources in a Resource Manager template.</span></span>

## <a name="defining-data-factory-resources-in-templates"></a><span data-ttu-id="2b153-134">Adat-előállító erőforrások meghatározása a sablonokban</span><span class="sxs-lookup"><span data-stu-id="2b153-134">Defining Data Factory resources in templates</span></span>
<span data-ttu-id="2b153-135">A legfelső szintű sablon meghatározásához egy adat-előállítót a következő:</span><span class="sxs-lookup"><span data-stu-id="2b153-135">The top-level template for defining a data factory is:</span></span>

```JSON
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
    { "type": "linkedservices",
        ...
    },
    {"type": "datasets",
        ...
    },
    {"type": "dataPipelines",
        ...
    }
}
```

### <a name="define-data-factory"></a><span data-ttu-id="2b153-136">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="2b153-136">Define data factory</span></span>
<span data-ttu-id="2b153-137">A data factoryt a Resource Manager-sablonban definiálhatja az alábbi minta szerint:</span><span class="sxs-lookup"><span data-stu-id="2b153-137">You define a data factory in the Resource Manager template as shown in the following sample:</span></span>

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
<span data-ttu-id="2b153-138">"Változó" a dataFactoryName típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="2b153-138">The dataFactoryName is defined in “variables” as:</span></span>

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a><span data-ttu-id="2b153-139">Adja meg a társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="2b153-139">Define linked services</span></span>

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

<span data-ttu-id="2b153-140">Lásd: [társított Társzolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) kívánja telepíteni az adott társított szolgáltatás JSON tulajdonságainak vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="2b153-140">See [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [Compute Linked Services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details about the JSON properties for the specific linked service you wish to deploy.</span></span> <span data-ttu-id="2b153-141">A "dependsOn" paraméternek megfelelő adat-előállító nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="2b153-141">The “dependsOn” parameter specifies name of the corresponding data factory.</span></span> <span data-ttu-id="2b153-142">Az Azure Storage összekapcsolt szolgáltatás meghatározásának példa az alábbi JSON-definícióban:</span><span class="sxs-lookup"><span data-stu-id="2b153-142">An example of defining a linked service for Azure Storage is shown in the following JSON definition:</span></span>

### <a name="define-datasets"></a><span data-ttu-id="2b153-143">Adja meg az adatkészletek</span><span class="sxs-lookup"><span data-stu-id="2b153-143">Define datasets</span></span>

```JSON
"type": "datasets",
"name": "[variables('<myDatasetName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<myDatasetLinkedServiceName>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    ...
}
```
<span data-ttu-id="2b153-144">Tekintse meg [adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) a JSON-tulajdonságok az adott adathalmaztípushoz vonatkozó további információért kívánja telepíteni.</span><span class="sxs-lookup"><span data-stu-id="2b153-144">Refer to [Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for details about the JSON properties for the specific dataset type you wish to deploy.</span></span> <span data-ttu-id="2b153-145">Megjegyzés: a "dependsOn" paraméter határozza meg a megfelelő adat-előállító és a tároló neve társított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="2b153-145">Note the “dependsOn” parameter specifies name of the corresponding data factory and storage linked service.</span></span> <span data-ttu-id="2b153-146">Az Azure blob-tároló adatkészlet típusú meghatározásának példa az alábbi JSON-definícióban:</span><span class="sxs-lookup"><span data-stu-id="2b153-146">An example of defining dataset type of Azure blob storage is shown in the following JSON definition:</span></span>

```JSON
"type": "datasets",
"name": "[variables('storageDataset')]",
"dependsOn": [
    "[variables('dataFactoryName')]",
    "[variables('storageLinkedServiceName')]"
],
"apiVersion": "2015-10-01",
"properties": {
"type": "AzureBlob",
"linkedServiceName": "[variables('storageLinkedServiceName')]",
"typeProperties": {
    "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
    "fileName": "[parameters('sourceBlobName')]",
    "format": {
        "type": "TextFormat"
    }
},
"availability": {
    "frequency": "Hour",
    "interval": 1
}
```

### <a name="define-pipelines"></a><span data-ttu-id="2b153-147">Folyamatok megadása</span><span class="sxs-lookup"><span data-stu-id="2b153-147">Define pipelines</span></span>

```JSON
"type": "dataPipelines",
"name": "[variables('<mypipelineName>')]",
"dependsOn": [
    "[variables('<dataFactoryName>')]",
    "[variables('<inputDatasetLinkedServiceName>')]",
    "[variables('<outputDatasetLinkedServiceName>')]",
    "[variables('<inputDataset>')]",
    "[variables('<outputDataset>')]"
],
"apiVersion": "2015-10-01",
"properties": {
    activities: {
        ...
    }
}
```

<span data-ttu-id="2b153-148">Tekintse meg [folyamatok meghatározása](data-factory-create-pipelines.md#pipeline-json) JSON tulajdonságainak meghatározása a megadott feldolgozási sorban lévő és a tevékenységek kívánja telepíteni vonatkozó további információért.</span><span class="sxs-lookup"><span data-stu-id="2b153-148">Refer to [defining pipelines](data-factory-create-pipelines.md#pipeline-json) for details about the JSON properties for defining the specific pipeline and activities you wish to deploy.</span></span> <span data-ttu-id="2b153-149">Jegyezze fel az "dependsOn" paraméter határozza meg a data factory nevét, és a szolgáltatások és adathalmazokat bármely megfelelő-e kapcsolva.</span><span class="sxs-lookup"><span data-stu-id="2b153-149">Note the “dependsOn” parameter specifies name of the data factory, and any corresponding linked services or datasets.</span></span> <span data-ttu-id="2b153-150">Egy folyamatot, amely adatainak másolása az Azure Blob Storage-ból az Azure SQL Database-példa az az alábbi JSON kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="2b153-150">An example of a pipeline that copies data from Azure Blob Storage to Azure SQL Database is shown in the following JSON snippet:</span></span>

```JSON
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
        "description": "Copy data frm Azure blob to Azure SQL",
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
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a><span data-ttu-id="2b153-151">Adat-előállító sablon paraméterezése</span><span class="sxs-lookup"><span data-stu-id="2b153-151">Parameterizing Data Factory template</span></span>
<span data-ttu-id="2b153-152">Gyakorlati tanácsok a paraméterezése, lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) cikk.</span><span class="sxs-lookup"><span data-stu-id="2b153-152">For best practices on parameterizing, see [Best practices for creating Azure Resource Manager templates](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) article.</span></span> <span data-ttu-id="2b153-153">Általában a paraméter használatával kell tartani, különösen akkor, ha a változók helyett használhatók.</span><span class="sxs-lookup"><span data-stu-id="2b153-153">In general, parameter usage should be minimized, especially if variables can be used instead.</span></span> <span data-ttu-id="2b153-154">Csak adja meg a paramétereket a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="2b153-154">Only provide parameters in the following scenarios:</span></span>

* <span data-ttu-id="2b153-155">Beállítások eltérőek lehetnek a környezet által (Példa: fejlesztői, tesztelési és éles)</span><span class="sxs-lookup"><span data-stu-id="2b153-155">Settings vary by environment (example: development, test, and production)</span></span>
* <span data-ttu-id="2b153-156">Titkos kulcsokat (például a jelszavak)</span><span class="sxs-lookup"><span data-stu-id="2b153-156">Secrets (such as passwords)</span></span>

<span data-ttu-id="2b153-157">Ha a titkos kulcsok lekéréses kell [Azure Key Vault](../key-vault/key-vault-get-started.md) Azure Data Factory entitások-sablonokkal való telepítésekor adja meg a **kulcstároló** és **titkos neve** látható módon a Példa:</span><span class="sxs-lookup"><span data-stu-id="2b153-157">If you need to pull secrets from [Azure Key Vault](../key-vault/key-vault-get-started.md) when deploying Azure Data Factory entities using templates, specify the **key vault** and **secret name** as shown in the following example:</span></span>

```JSON
"parameters": {
    "storageAccountKey": {
        "reference": {
            "keyVault": {
                "id":"/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>/providers/Microsoft.KeyVault/vaults/<keyVaultName>",
             },
            "secretName": "<secretName>"
           },
       },
       ...
}
```

> [!NOTE]
> <span data-ttu-id="2b153-158">A meglévő adat-előállítók sablonok exportálása jelenleg még nem támogatott, de napjainkban működik.</span><span class="sxs-lookup"><span data-stu-id="2b153-158">While exporting templates for existing data factories is currently not yet supported, it is in the works.</span></span>
>
>
