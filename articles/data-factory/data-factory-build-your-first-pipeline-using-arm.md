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
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-resource-manager-template"></a>Oktatóanyag: Az első Azure data factory létrehozása Azure Resource Manager-sablon használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
> 
> 

Ebben a cikkben használhatja az Azure Resource Manager sablon toocreate az első az Azure data factory. toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.

Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**. Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt. hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után. 

> [!NOTE]
> hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Ebben az oktatóanyagban hello csővezeték típusú csak egy tevékenység rendelkezik: HDInsightHive. Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

## <a name="prerequisites"></a>Előfeltételek
* Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.
* Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen.
* Lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Azure Resource Manager-sablonok. 

## <a name="in-this-tutorial"></a>Az oktatóanyag tartalma
| Entitás | Leírás |
| --- | --- |
| Azure Storage társított szolgáltatás |Az Azure Storage-fiók toohello adat-előállító hivatkozásokat tartalmaz. hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók. |
| HDInsight igény szerinti társított szolgáltatás |Egy igény szerinti HDInsight fürt toohello adat-előállító hivatkozásokat tartalmaz. hello fürt automatikusan létrejön tooprocess adatokat, és hello feldolgozás befejezése után törlődik. |
| Azure Blob bemeneti adatkészlet |Toohello Azure tárolás társított szolgáltatásának hivatkozik. hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello bemeneti adatokat tartalmazó hello tárolóban. |
| Azure Blob kimeneti adatkészlet |Toohello Azure tárolás társított szolgáltatásának hivatkozik. hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello kimeneti adatokat tartalmazó hello tárolóban. |
| Adatfolyamat |hello folyamat rendelkezik egy tevékenység típusú HDInsightHive, amely hello bemeneti adatkészlet-t használ fel, és létrehozza hello kimeneti adatkészletet. |

A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Kétféle típusú tevékenység létezik: az [adattovábbítási tevékenységek](data-factory-data-movement-activities.md) és az [adatátalakítási tevékenységek](data-factory-data-transformation-activities.md). Az oktatóanyag segítségével egyetlen tevékenységgel (Hive-tevékenységgel) rendelkező folyamatot hozhat létre.

hello következő ismertetett hello teljes Resource Manager-sablon adat-előállító entitások meghatározása, hogy gyorsan hello oktatóanyag és tesztelési hello sablon segítségével futtathatja. toounderstand hogyan minden adat-előállító entitáshoz van definiálva, lásd: [adat-előállító entitások hello sablonban](#data-factory-entities-in-the-template) szakasz.

## <a name="data-factory-json-template"></a>Data Factory JSON-sablon
hello legfelső szintű erőforrás-kezelő sablon meghatározásához egy adat-előállítót a következő: 

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
Hozzon létre egy JSON fájlt **ADFTutorialARM.json** a **C:\ADFGetStarted** hello tartalom a következő mappában:

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
> Az Azure data factory létrehozására szolgáló Resource Manager-sablonra a következő helyen találhat egy másik példát: [Oktatóanyag: Folyamat létrehozása másolási tevékenységgel és Azure Resource Manager-sablon használatával](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md).  
> 
> 

## <a name="parameters-json"></a>Paramétereket tartalmazó JSON-file
Hozzon létre egy JSON fájlt **ADFTutorialARM-Parameters.json** tartalmazó hello Azure Resource Manager sablon paramétereit.  

> [!IMPORTANT]
> Adja meg a hello neve és kulcsa az Azure Storage-fiókjának hello **storageAccountName** és **storageAccountKey** paraméterek paraméter fájlban. 
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
> Előfordulhat, hogy külön paraméter JSON-fájlok fejlesztési, tesztelési, és csak akkor használhatja az üzemi környezetek hello azonos Data Factory JSON-sablon. Ezekben a környezetekben automatizálhatja a Data Factory-entitások üzembe helyezését egy PowerShell-szkript használatával. 
> 
> 

## <a name="create-data-factory"></a>Data factory létrehozása
1. Start **Azure PowerShell** és futtatási hello a következő parancsot: 
   * Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.
    ```PowerShell
    Login-AzureRmAccount
    ```  
   * Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.
    ```PowerShell
    Get-AzureRmSubscription
    ``` 
   * Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello. Ez az előfizetés ugyanaz, mint egy Azure-portálon hello használt hello kell hello.
    ```
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```   
2. Futtassa a következő parancs toodeploy adat-előállító entitások 1. lépésben létrehozott hello Resource Manager-sablon használatával hello. 

    ```PowerShell
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Folyamat figyelése
1. Toohello a bejelentkezés után [Azure-portálon](https://portal.azure.com/), kattintson a **Tallózás** válassza **adat-előállítók**.
     ![Tallózás-&gt;Data Factories](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2. A hello **adat-előállítók** panelen kattintson az adat-előállító hello (**TutorialFactoryARM**) alapján létrehozott.    
3. A hello **adat-előállító** a data factory paneljén kattintson **Diagram**.

     ![Diagram csempe](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4. A hello **diagramnézet**, az hello adatcsatornák áttekintés és adatkészletek szerepel ez az oktatóanyag.
   
   ![Diagramnézet](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
5. A Diagram nézet hello, kattintson duplán a hello dataset **AzureBlobOutput**. Láthatja, hogy hello szelet, amely folyamatban van.
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
6. Ha nem hajtja végre, megjelenik az hello szeletre **készen** állapotát. Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.
   
    ![Adatkészlet](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png)    
7. Ha hello szelet belül van **készen áll a** állapot, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.  

Lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) hogyan toouse hello Azure portál paneleken toomonitor hello feldolgozási sorban lévő és adatkészletek létrehozott ebben az oktatóanyagban kapcsolatos utasításokat.

Figyelő és App kezelése toomonitor az adatok folyamatok is használja. Lásd: [figyelése és kezelése az Azure Data Factory folyamatok figyelése alkalmazással](data-factory-monitor-manage-app.md) hello alkalmazás használatával kapcsolatos. 

> [!IMPORTANT]
> hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént. Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.
> 
> 

## <a name="data-factory-entities-in-hello-template"></a>Data Factory entitások hello sablonban
### <a name="define-data-factory"></a>Data Factory definiálása
Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```
hello dataFactoryName típusúként van definiálva: 

```json
"dataFactoryName": "[concat('HiveTransformDF', uniqueString(resourceGroup().id))]",
```
Egy egyedi karakterlánc alapján hello erőforrás azonosítót.  

### <a name="defining-data-factory-entities"></a>Data Factory-entitások definiálása
hello következő adat-előállító entitások definiált hello JSON-sablon: 

* [Azure Storage társított szolgáltatás](#azure-storage-linked-service)
* [HDInsight igény szerinti társított szolgáltatás](#hdinsight-on-demand-linked-service)
* [Azure blobbemeneti adatkészlet](#azure-blob-input-dataset)
* [Azure blobkimeneti adatkészlet](#azure-blob-output-dataset)
* [Másolási tevékenységgel rendelkező adatfolyamat](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát. Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak. 

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
Hello **connectionString** használ hello storageAccountName és storageAccountKey paraméterek. Ezek a paraméterek az átadott konfigurációs fájl használatával hello értékeit. hello definition is változókat használ: hello sablonban meghatározott azureStroageLinkedService és dataFactoryName. 

#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight igény szerinti társított szolgáltatás
Lásd: [összekapcsolt szolgáltatások számítási](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért cikk egy HDInsight igény társított szolgáltatást.  

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
Vegye figyelembe a következő pontok hello: 

* hello adat-előállító létrehoz egy **Linux-alapú** a fenti JSON hello meg HDInsight-fürthöz. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás). 
* Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat. További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).
* hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz toobe (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.
  
    Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).

#### <a name="azure-blob-input-dataset"></a>Azure blobbemeneti adatkészlet
Megadhatja a blob-tároló, a mappa és a hello bemeneti adatokat tartalmazó fájlt hello nevét. Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért. 

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
Ez a definíció használja a következő paraméter sablonban definiált paraméterek hello: blobContainer, inputBlobFolder, és inputBlobName. 

#### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet
Megadhatja a hello blobtárolót és hello kimeneti adatokat tartalmazó mappa nevét. Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.  

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

Ez a definíció használja a következő paraméterek hello paraméter sablonban meghatározott hello: blobContainer és outputBlobFolder. 

#### <a name="data-pipeline"></a>Adatfolyamat
Definiálhat egy folyamatot, amely átalakítja az adatokat a Hive-parancsfájl egy igény szerinti Azure HDInsight-fürtön való futtatásával. Lásd: [adatcsatorna JSON](data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását. 

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

## <a name="reuse-hello-template"></a>Újbóli hello sablon
Hello oktatóanyagban létrehozott egy sablont a Data Factory entitásokat és a paraméterek értékeit, hogy a sablon meghatározásához. toouse hello egyező sablon toodeploy adat-előállító entitások toodifferent környezetekhez, hozzon létre egy paraméter környezetben, és használatra, ha toothat környezet telepítése.     

Példa:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Dev.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Test.json

New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFTutorialARM.json -TemplateParameterFile ADFTutorialARM-Parameters-Production.json
```
Figyelje meg, hogy az első parancs által használt paraméter hello hello fejlesztői környezetben hello tesztkörnyezetben, a másikat a fájlt, és hello harmadik egy hello éles környezetben.  

Hello sablon tooperform is felhasználhatja ismétlődő feladatokat. Például egy sok adat-előállítók kell toocreate, vagy további folyamatok, amelyek megvalósítják hello ugyanez a logika, de minden adatok gyári által használt különböző Azure storage és az Azure SQL Database-fiókok. Ebben a forgatókönyvben hello használhatja ugyanazt a sablont a hello ugyanabban a környezetben (fejlesztői, tesztelési vagy éles) különböző paraméterrel toocreate adat-előállítók fájlok. 

## <a name="resource-manager-template-for-creating-a-gateway"></a>Resource Manager-sablon átjáró létrehozásához
Íme egy minta logikai átjáró létrehozásához hello vissza a Resource Manager-sablon. Átjáró telepítéséhez a helyi számítógépen vagy Azure IaaS virtuális Gépen, és hello átjáró regisztrálása kulccsal Data Factory szolgáltatásban. További információkért lásd: [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) (Adatok áthelyezése a helyszíni rendszer és a felhő között).

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
Ez a sablon létrehozza a GatewayUsingArmDF nevű data factoryt a GatewayUsingARM nevű átjáróval. 

## <a name="see-also"></a>Lásd még:
| Témakör | Leírás |
|:--- |:--- |
| [Folyamatok](data-factory-create-pipelines.md) |Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás. |

