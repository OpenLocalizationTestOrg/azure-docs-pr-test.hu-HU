---
title: a Data Factory aaaUse Resource Manager-sablonok |} Microsoft Docs
description: "Megtudhatja, hogyan toocreate és használata Azure Resource Manager sablonok toocreate adat-előállító entitásokat."
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
ms.openlocfilehash: 60d5dbd29494420006aed6d5bd9a10a63c36bec3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-templates-toocreate-azure-data-factory-entities"></a><span data-ttu-id="9100e-103">Sablonok toocreate Azure Data Factory entitások használata</span><span class="sxs-lookup"><span data-stu-id="9100e-103">Use templates toocreate Azure Data Factory entities</span></span>
## <a name="overview"></a><span data-ttu-id="9100e-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="9100e-104">Overview</span></span>
<span data-ttu-id="9100e-105">Során azt tapasztalhatja, saját maga az adat-integráció igényeinek Azure Data Factory használatával újból felhasználja a hello ugyanilyen mintájú különböző környezetek között, vagy hello végrehajtási ugyanazt a feladatot többször is belül hello azonos megoldás.</span><span class="sxs-lookup"><span data-stu-id="9100e-105">While using Azure Data Factory for your data integration needs, you may find yourself reusing hello same pattern across different environments or implementing hello same task repetitively within hello same solution.</span></span> <span data-ttu-id="9100e-106">A sablonok révén megvalósítása és kezelése forgatókönyvekben egyszerű módon.</span><span class="sxs-lookup"><span data-stu-id="9100e-106">Templates help you implement and manage these scenarios in an easy manner.</span></span> <span data-ttu-id="9100e-107">Sablonok az Azure Data Factory ideálisak újrahasznosításának és ismétlődési forgatókönyveit.</span><span class="sxs-lookup"><span data-stu-id="9100e-107">Templates in Azure Data Factory are ideal for scenarios that involve reusability and repetition.</span></span>

<span data-ttu-id="9100e-108">Vegye figyelembe, ha egy szervezet rendelkezik a 10 gyártási tőzsdei árfolyamjelző között hello world hello helyzet.</span><span class="sxs-lookup"><span data-stu-id="9100e-108">Consider hello situation where an organization has 10 manufacturing plants across hello world.</span></span> <span data-ttu-id="9100e-109">az egyes gépek hello naplók önálló helyszíni SQL Server-adatbázisban tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="9100e-109">hello logs from each plant are stored in a separate on-premises SQL Server database.</span></span> <span data-ttu-id="9100e-110">hello vállalat hello felhőben egyetlen adatraktár szeretne toobuild alkalmi elemzéséhez.</span><span class="sxs-lookup"><span data-stu-id="9100e-110">hello company wants toobuild a single data warehouse in hello cloud for ad-hoc analytics.</span></span> <span data-ttu-id="9100e-111">Azt is szeretné toohave hello ugyanez a logika, de különböző konfigurációt fejlesztői, tesztelési és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9100e-111">It also wants toohave hello same logic but different configurations for development, test, and production environments.</span></span>

<span data-ttu-id="9100e-112">Ebben az esetben a feladat ismételt toobe kell belül hello ugyanabban a környezetben, de eltérő értékű között hello minden olyan üzem 10 adat-előállítók.</span><span class="sxs-lookup"><span data-stu-id="9100e-112">In this case, a task needs toobe repeated within hello same environment, but with different values across hello 10 data factories for each manufacturing plant.</span></span> <span data-ttu-id="9100e-113">Gyakorlatilag **ismétlődési** található.</span><span class="sxs-lookup"><span data-stu-id="9100e-113">In effect, **repetition** is present.</span></span> <span data-ttu-id="9100e-114">Templating lehetővé teszi, hogy az általános folyamat hello elvonása (Ez azt jelenti, hogy folyamatok rendelkező hello minden adat-előállítóban azonos tevékenységek), de egy különálló paraméterfájl is használnak az minden olyan üzem.</span><span class="sxs-lookup"><span data-stu-id="9100e-114">Templating allows hello abstraction of this generic flow (that is, pipelines having hello same activities in each data factory), but uses a separate parameter file for each manufacturing plant.</span></span>

<span data-ttu-id="9100e-115">Ezenkívül hello szervezet által toodeploy ezek 10 adat-előállítók többször különböző környezetek között, sablonok használatával Ez **újrahasznosításának** külön paraméterfájlt fejlesztésekhez felügyelniük teszteléséhez és éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="9100e-115">Furthermore, as hello organization wants toodeploy these 10 data factories multiple times across different environments, templates can use this **reusability** by utilizing separate parameter files for development, test, and production environments.</span></span>

## <a name="templating-with-azure-resource-manager"></a><span data-ttu-id="9100e-116">Az Azure Resource Manager eszközzel templating</span><span class="sxs-lookup"><span data-stu-id="9100e-116">Templating with Azure Resource Manager</span></span>
<span data-ttu-id="9100e-117">[Az Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md#template-deployment) egy kiváló módja tooachieve templating az Azure Data Factory vannak.</span><span class="sxs-lookup"><span data-stu-id="9100e-117">[Azure Resource Manager templates](../azure-resource-manager/resource-group-overview.md#template-deployment) are a great way tooachieve templating in Azure Data Factory.</span></span> <span data-ttu-id="9100e-118">Resource Manager-sablonok meghatározott hello infrastruktúráját és konfigurációját az Azure-megoldás a JSON-fájl.</span><span class="sxs-lookup"><span data-stu-id="9100e-118">Resource Manager templates define hello infrastructure and configuration of your Azure solution through a JSON file.</span></span> <span data-ttu-id="9100e-119">Mivel az Azure Resource Manager-sablonok összes/legtöbb Azure-szolgáltatások használatához széles körben használható tooeasily kezelése az Azure eszközök összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="9100e-119">Because Azure Resource Manager templates work with all/most Azure services, it can be widely used tooeasily manage all resources of your Azure assets.</span></span> <span data-ttu-id="9100e-120">Lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn arról hello Resource Manager-sablonok általában.</span><span class="sxs-lookup"><span data-stu-id="9100e-120">See [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md) toolearn more about hello Resource Manager Templates in general.</span></span>

## <a name="tutorials"></a><span data-ttu-id="9100e-121">oktatóanyagokat</span><span class="sxs-lookup"><span data-stu-id="9100e-121">Tutorials</span></span>
<span data-ttu-id="9100e-122">Lásd: hello oktatóanyagok részletesen toocreate adat-előállító entitások Resource Manager-sablonok használatával a következő:</span><span class="sxs-lookup"><span data-stu-id="9100e-122">See hello following tutorials for step-by-step instructions toocreate Data Factory entities by using Resource Manager templates:</span></span>

* [<span data-ttu-id="9100e-123">Oktatóanyag: A folyamat toocopy adatok létrehozásához használja az Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9100e-123">Tutorial: Create a pipeline toocopy data by using Azure Resource Manager template</span></span>](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [<span data-ttu-id="9100e-124">Oktatóanyag: A folyamat tooprocess adatok létrehozásához használja az Azure Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="9100e-124">Tutorial: Create a pipeline tooprocess data by using Azure Resource Manager template</span></span>](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a><span data-ttu-id="9100e-125">Data Factory sablonok a Githubon</span><span class="sxs-lookup"><span data-stu-id="9100e-125">Data Factory templates on GitHub</span></span>
<span data-ttu-id="9100e-126">Tekintse meg a következő Azure gyors üzembe helyezési sablonokat a Githubon hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-126">Check out hello following Azure quick start templates on GitHub:</span></span>

* [<span data-ttu-id="9100e-127">A Data factory toocopy adatok Azure Blob Storage tooAzure SQL-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="9100e-127">Create a Data factory toocopy data from Azure Blob Storage tooAzure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [<span data-ttu-id="9100e-128">Hozzon létre egy adat-előállító Hive tevékenység on Azure HDInsight-fürt</span><span class="sxs-lookup"><span data-stu-id="9100e-128">Create a Data factory with Hive activity on Azure HDInsight cluster</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [<span data-ttu-id="9100e-129">A Data factory toocopy adatok Salesforce tooAzure Blobok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9100e-129">Create a Data factory toocopy data from Salesforce tooAzure Blobs</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [<span data-ttu-id="9100e-130">Tevékenységek láncában egy adat-előállító létrehozása: másol adatokat az FTP-kiszolgáló tooAzure Blobok hív meg, a hive parancsfájl egy igény szerinti HDInsight fürt tootransform hello adatokon, és másolja át az eredményt az Azure SQL-adatbázisba</span><span class="sxs-lookup"><span data-stu-id="9100e-130">Create a Data factory that chains activities: copies data from an FTP server tooAzure Blobs, invokes a hive script on an on-demand HDInsight cluster tootransform hello data, and copies result into Azure SQL Database</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

<span data-ttu-id="9100e-131">Érzi, hogy a szabad tooshare az Azure Data Factory sablonokat [Azure gyors üzembe helyezési](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="9100e-131">Feel free tooshare your Azure Data Factory templates at [Azure Quick start](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="9100e-132">Tekintse meg a toohello [hozzájárulás útmutató](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) sablonok, amelyek megoszthatók az ebben a tárházban keresztül fejlesztése során.</span><span class="sxs-lookup"><span data-stu-id="9100e-132">Refer toohello [contribution guide](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) while developing templates that can be shared via this repository.</span></span>

<span data-ttu-id="9100e-133">a következő szakaszok hello adat-előállító erőforrások adjon meg egy Resource Manager-sablon a részleteit tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="9100e-133">hello following sections provide details about defining Data Factory resources in a Resource Manager template.</span></span>

## <a name="defining-data-factory-resources-in-templates"></a><span data-ttu-id="9100e-134">Adat-előállító erőforrások meghatározása a sablonokban</span><span class="sxs-lookup"><span data-stu-id="9100e-134">Defining Data Factory resources in templates</span></span>
<span data-ttu-id="9100e-135">hello legfelső szintű sablon meghatározásához egy adat-előállítót a következő:</span><span class="sxs-lookup"><span data-stu-id="9100e-135">hello top-level template for defining a data factory is:</span></span>

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

### <a name="define-data-factory"></a><span data-ttu-id="9100e-136">Data Factory definiálása</span><span class="sxs-lookup"><span data-stu-id="9100e-136">Define data factory</span></span>
<span data-ttu-id="9100e-137">Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-137">You define a data factory in hello Resource Manager template as shown in hello following sample:</span></span>

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
<span data-ttu-id="9100e-138">"változó" Hello dataFactoryName típusúként van definiálva:</span><span class="sxs-lookup"><span data-stu-id="9100e-138">hello dataFactoryName is defined in “variables” as:</span></span>

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a><span data-ttu-id="9100e-139">Adja meg a társított szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9100e-139">Define linked services</span></span>

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

<span data-ttu-id="9100e-140">Lásd: [társított Társzolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) hello adott társított szolgáltatás JSON tulajdonságai hello vonatkozó további információért toodeploy kívánja.</span><span class="sxs-lookup"><span data-stu-id="9100e-140">See [Storage Linked Service](data-factory-azure-blob-connector.md#azure-storage-linked-service) or [Compute Linked Services](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) for details about hello JSON properties for hello specific linked service you wish toodeploy.</span></span> <span data-ttu-id="9100e-141">hello "dependsOn" paraméter hello megfelelő adat-előállító nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="9100e-141">hello “dependsOn” parameter specifies name of hello corresponding data factory.</span></span> <span data-ttu-id="9100e-142">Az Azure Storage összekapcsolt szolgáltatás meghatározásának példa a következő JSON-definícióból hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-142">An example of defining a linked service for Azure Storage is shown in hello following JSON definition:</span></span>

### <a name="define-datasets"></a><span data-ttu-id="9100e-143">Adja meg az adatkészletek</span><span class="sxs-lookup"><span data-stu-id="9100e-143">Define datasets</span></span>

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
<span data-ttu-id="9100e-144">Tekintse meg a túl[adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) hello JSON tulajdonságok hello adott adathalmaztípushoz vonatkozó további információért toodeploy kívánja.</span><span class="sxs-lookup"><span data-stu-id="9100e-144">Refer too[Supported data stores](data-factory-data-movement-activities.md#supported-data-stores-and-formats) for details about hello JSON properties for hello specific dataset type you wish toodeploy.</span></span> <span data-ttu-id="9100e-145">Megjegyzés: hello "dependsOn" paraméter határozza meg a megfelelő adatok hello neve gyári és tárolás társított szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="9100e-145">Note hello “dependsOn” parameter specifies name of hello corresponding data factory and storage linked service.</span></span> <span data-ttu-id="9100e-146">Az Azure blob-tároló adatkészlet típusú meghatározásának példa a következő JSON-definícióból hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-146">An example of defining dataset type of Azure blob storage is shown in hello following JSON definition:</span></span>

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

### <a name="define-pipelines"></a><span data-ttu-id="9100e-147">Folyamatok megadása</span><span class="sxs-lookup"><span data-stu-id="9100e-147">Define pipelines</span></span>

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

<span data-ttu-id="9100e-148">Tekintse meg a túl[folyamatok meghatározása](data-factory-create-pipelines.md#pipeline-json) a hello JSON tulajdonságok meghatározásához adatait hello adott prognózisokat és tevékenységeket toodeploy kívánja.</span><span class="sxs-lookup"><span data-stu-id="9100e-148">Refer too[defining pipelines](data-factory-create-pipelines.md#pipeline-json) for details about hello JSON properties for defining hello specific pipeline and activities you wish toodeploy.</span></span> <span data-ttu-id="9100e-149">Megjegyzés: hello "dependsOn" paraméter neve hello adat-előállítót, és bármely megfelelő társított szolgáltatások és adathalmazokat.</span><span class="sxs-lookup"><span data-stu-id="9100e-149">Note hello “dependsOn” parameter specifies name of hello data factory, and any corresponding linked services or datasets.</span></span> <span data-ttu-id="9100e-150">Egy folyamatot, amely az Azure Blob Storage tooAzure SQL-adatbázis adatainak másolása példa az alábbi JSON részlet hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-150">An example of a pipeline that copies data from Azure Blob Storage tooAzure SQL Database is shown in hello following JSON snippet:</span></span>

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
    "start": "2016-10-03T00:00:00Z",
    "end": "2016-10-04T00:00:00Z"
}
```
## <a name="parameterizing-data-factory-template"></a><span data-ttu-id="9100e-151">Adat-előállító sablon paraméterezése</span><span class="sxs-lookup"><span data-stu-id="9100e-151">Parameterizing Data Factory template</span></span>
<span data-ttu-id="9100e-152">Gyakorlati tanácsok a paraméterezése, lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) cikk.</span><span class="sxs-lookup"><span data-stu-id="9100e-152">For best practices on parameterizing, see [Best practices for creating Azure Resource Manager templates](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) article.</span></span> <span data-ttu-id="9100e-153">Általában a paraméter használatával kell tartani, különösen akkor, ha a változók helyett használhatók.</span><span class="sxs-lookup"><span data-stu-id="9100e-153">In general, parameter usage should be minimized, especially if variables can be used instead.</span></span> <span data-ttu-id="9100e-154">Csak olyan hello forgatókönyvek a következő paramétereket:</span><span class="sxs-lookup"><span data-stu-id="9100e-154">Only provide parameters in hello following scenarios:</span></span>

* <span data-ttu-id="9100e-155">Beállítások eltérőek lehetnek a környezet által (Példa: fejlesztői, tesztelési és éles)</span><span class="sxs-lookup"><span data-stu-id="9100e-155">Settings vary by environment (example: development, test, and production)</span></span>
* <span data-ttu-id="9100e-156">Titkos kulcsokat (például a jelszavak)</span><span class="sxs-lookup"><span data-stu-id="9100e-156">Secrets (such as passwords)</span></span>

<span data-ttu-id="9100e-157">Ha a toopull titkok kell [Azure Key Vault](../key-vault/key-vault-get-started.md) Azure Data Factory entitások-sablonokkal való telepítésekor adja meg a hello **kulcstároló** és **titkos neve** látható módon a következő példa hello:</span><span class="sxs-lookup"><span data-stu-id="9100e-157">If you need toopull secrets from [Azure Key Vault](../key-vault/key-vault-get-started.md) when deploying Azure Data Factory entities using templates, specify hello **key vault** and **secret name** as shown in hello following example:</span></span>

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
> <span data-ttu-id="9100e-158">A meglévő adat-előállítók sablonok exportálása jelenleg még nem támogatott, de napjainkban hello Works.</span><span class="sxs-lookup"><span data-stu-id="9100e-158">While exporting templates for existing data factories is currently not yet supported, it is in hello works.</span></span>
>
>
