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
# <a name="use-templates-toocreate-azure-data-factory-entities"></a>Sablonok toocreate Azure Data Factory entitások használata
## <a name="overview"></a>Áttekintés
Során azt tapasztalhatja, saját maga az adat-integráció igényeinek Azure Data Factory használatával újból felhasználja a hello ugyanilyen mintájú különböző környezetek között, vagy hello végrehajtási ugyanazt a feladatot többször is belül hello azonos megoldás. A sablonok révén megvalósítása és kezelése forgatókönyvekben egyszerű módon. Sablonok az Azure Data Factory ideálisak újrahasznosításának és ismétlődési forgatókönyveit.

Vegye figyelembe, ha egy szervezet rendelkezik a 10 gyártási tőzsdei árfolyamjelző között hello world hello helyzet. az egyes gépek hello naplók önálló helyszíni SQL Server-adatbázisban tárolódnak. hello vállalat hello felhőben egyetlen adatraktár szeretne toobuild alkalmi elemzéséhez. Azt is szeretné toohave hello ugyanez a logika, de különböző konfigurációt fejlesztői, tesztelési és éles környezetben.

Ebben az esetben a feladat ismételt toobe kell belül hello ugyanabban a környezetben, de eltérő értékű között hello minden olyan üzem 10 adat-előállítók. Gyakorlatilag **ismétlődési** található. Templating lehetővé teszi, hogy az általános folyamat hello elvonása (Ez azt jelenti, hogy folyamatok rendelkező hello minden adat-előállítóban azonos tevékenységek), de egy különálló paraméterfájl is használnak az minden olyan üzem.

Ezenkívül hello szervezet által toodeploy ezek 10 adat-előállítók többször különböző környezetek között, sablonok használatával Ez **újrahasznosításának** külön paraméterfájlt fejlesztésekhez felügyelniük teszteléséhez és éles környezetben.

## <a name="templating-with-azure-resource-manager"></a>Az Azure Resource Manager eszközzel templating
[Az Azure Resource Manager-sablonok](../azure-resource-manager/resource-group-overview.md#template-deployment) egy kiváló módja tooachieve templating az Azure Data Factory vannak. Resource Manager-sablonok meghatározott hello infrastruktúráját és konfigurációját az Azure-megoldás a JSON-fájl. Mivel az Azure Resource Manager-sablonok összes/legtöbb Azure-szolgáltatások használatához széles körben használható tooeasily kezelése az Azure eszközök összes erőforrást. Lásd: [Azure Resource Manager-sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn arról hello Resource Manager-sablonok általában.

## <a name="tutorials"></a>oktatóanyagokat
Lásd: hello oktatóanyagok részletesen toocreate adat-előállító entitások Resource Manager-sablonok használatával a következő:

* [Oktatóanyag: A folyamat toocopy adatok létrehozásához használja az Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [Oktatóanyag: A folyamat tooprocess adatok létrehozásához használja az Azure Resource Manager-sablon](data-factory-build-your-first-pipeline.md)

## <a name="data-factory-templates-on-github"></a>Data Factory sablonok a Githubon
Tekintse meg a következő Azure gyors üzembe helyezési sablonokat a Githubon hello:

* [A Data factory toocopy adatok Azure Blob Storage tooAzure SQL-adatbázis létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-blob-to-sql-copy)
* [Hozzon létre egy adat-előállító Hive tevékenység on Azure HDInsight-fürt](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-hive-transformation)
* [A Data factory toocopy adatok Salesforce tooAzure Blobok létrehozása](https://github.com/Azure/azure-quickstart-templates/tree/master/101-data-factory-salesforce-to-blob-copy)
* [Tevékenységek láncában egy adat-előállító létrehozása: másol adatokat az FTP-kiszolgáló tooAzure Blobok hív meg, a hive parancsfájl egy igény szerinti HDInsight fürt tootransform hello adatokon, és másolja át az eredményt az Azure SQL-adatbázisba](https://github.com/Azure/azure-quickstart-templates/tree/master/201-data-factory-ftp-hive-blob)

Érzi, hogy a szabad tooshare az Azure Data Factory sablonokat [Azure gyors üzembe helyezési](https://azure.microsoft.com/documentation/templates/). Tekintse meg a toohello [hozzájárulás útmutató](https://github.com/Azure/azure-quickstart-templates/tree/master/1-CONTRIBUTION-GUIDE) sablonok, amelyek megoszthatók az ebben a tárházban keresztül fejlesztése során.

a következő szakaszok hello adat-előállító erőforrások adjon meg egy Resource Manager-sablon a részleteit tartalmazzák.

## <a name="defining-data-factory-resources-in-templates"></a>Adat-előállító erőforrások meghatározása a sablonokban
hello legfelső szintű sablon meghatározásához egy adat-előállítót a következő:

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

### <a name="define-data-factory"></a>Data Factory definiálása
Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:

```JSON
"resources": [
{
    "name": "[variables('<mydataFactoryName>')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "East US"
}
```
"változó" Hello dataFactoryName típusúként van definiálva:

```JSON
"dataFactoryName": "[concat('<myDataFactoryName>', uniqueString(resourceGroup().id))]",
```

### <a name="define-linked-services"></a>Adja meg a társított szolgáltatások

```JSON
"type": "linkedservices",
"name": "[variables('<LinkedServiceName>')]",
"apiVersion": "2015-10-01",
"dependsOn": [ "[variables('<dataFactoryName>')]" ],
"properties": {
    ...
}
```

Lásd: [társított Társzolgáltatás](data-factory-azure-blob-connector.md#azure-storage-linked-service) vagy [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) hello adott társított szolgáltatás JSON tulajdonságai hello vonatkozó további információért toodeploy kívánja. hello "dependsOn" paraméter hello megfelelő adat-előállító nevét adja meg. Az Azure Storage összekapcsolt szolgáltatás meghatározásának példa a következő JSON-definícióból hello:

### <a name="define-datasets"></a>Adja meg az adatkészletek

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
Tekintse meg a túl[adattárolókhoz támogatott](data-factory-data-movement-activities.md#supported-data-stores-and-formats) hello JSON tulajdonságok hello adott adathalmaztípushoz vonatkozó további információért toodeploy kívánja. Megjegyzés: hello "dependsOn" paraméter határozza meg a megfelelő adatok hello neve gyári és tárolás társított szolgáltatása. Az Azure blob-tároló adatkészlet típusú meghatározásának példa a következő JSON-definícióból hello:

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

### <a name="define-pipelines"></a>Folyamatok megadása

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

Tekintse meg a túl[folyamatok meghatározása](data-factory-create-pipelines.md#pipeline-json) a hello JSON tulajdonságok meghatározásához adatait hello adott prognózisokat és tevékenységeket toodeploy kívánja. Megjegyzés: hello "dependsOn" paraméter neve hello adat-előállítót, és bármely megfelelő társított szolgáltatások és adathalmazokat. Egy folyamatot, amely az Azure Blob Storage tooAzure SQL-adatbázis adatainak másolása példa az alábbi JSON részlet hello:

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
## <a name="parameterizing-data-factory-template"></a>Adat-előállító sablon paraméterezése
Gyakorlati tanácsok a paraméterezése, lásd: [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../azure-resource-manager/resource-manager-template-best-practices.md#parameters) cikk. Általában a paraméter használatával kell tartani, különösen akkor, ha a változók helyett használhatók. Csak olyan hello forgatókönyvek a következő paramétereket:

* Beállítások eltérőek lehetnek a környezet által (Példa: fejlesztői, tesztelési és éles)
* Titkos kulcsokat (például a jelszavak)

Ha a toopull titkok kell [Azure Key Vault](../key-vault/key-vault-get-started.md) Azure Data Factory entitások-sablonokkal való telepítésekor adja meg a hello **kulcstároló** és **titkos neve** látható módon a következő példa hello:

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
> A meglévő adat-előállítók sablonok exportálása jelenleg még nem támogatott, de napjainkban hello Works.
>
>
