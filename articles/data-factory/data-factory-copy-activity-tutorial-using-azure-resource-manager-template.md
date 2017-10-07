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
# <a name="tutorial-use-azure-resource-manager-template-toocreate-a-data-factory-pipeline-toocopy-data"></a>Oktatóanyag: Használata Azure Resource Manager sablon toocreate egy adat-előállító adatcsatorna toocopy adatok 
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Másolás varázsló](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sablon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toocreate egy az Azure data factory. hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Nem alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md).

Az oktatóanyag segítségével egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot hozhat létre. hello másolási tevékenység során a támogatott adatokat tároló tooa támogatott fogadó adatokat tároló másol adatokat. A forrásként és fogadóként támogatott adattárak listájáért lásd: [támogatott adattárak](data-factory-data-movement-activities.md#supported-data-stores-and-formats). hello tevékenység egy globálisan elérhető szolgáltatás közötti biztonságos, megbízható és skálázható módon különböző adattárolókhoz másolhatja van-e kapcsolva. További információ a másolási tevékenység hello: [adatok mozgása tevékenységek](data-factory-data-movement-activities.md).

Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További információért lásd: [egy folyamaton belüli több tevékenység](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline). 

> [!NOTE] 
> hello adatok feldolgozási sor az oktatóanyag a forrás adatokat tároló tooa cél adatokat tároló másol adatokat. Hogyan oktatóanyagért tootransform adatok Azure Data Factory használatával, lásd: [oktatóanyag: a folyamat tootransform adatokat Hadoop-fürt létrehozása](data-factory-build-your-first-pipeline.md). 

## <a name="prerequisites"></a>Előfeltételek
* Lépkedjen végig [oktatóanyag – áttekintés és előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) és teljes hello **előfeltétel** lépéseket.
* Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen. Ebben az oktatóanyagban a PowerShell toodeploy adat-előállító entitások használja. 
* (választható) Lásd: [Azure Resource Manager sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md) toolearn Azure Resource Manager-sablonok.

## <a name="in-this-tutorial"></a>Az oktatóanyag tartalma
Ebben az oktatóanyagban a Data Factory entitások következő hello hoz létre egy adat-előállító:

| Entitás | Leírás |
| --- | --- |
| Azure Storage társított szolgáltatás |Az Azure Storage-fiók toohello adat-előállító hivatkozásokat tartalmaz. Az Azure Storage hello forrás adattár pedig Azure SQL adatbázis hello fogadó adattár hello másolási tevékenységhez hello oktatóanyag. Azt adja meg a hello másolási tevékenységhez hello a bemeneti adatokat tartalmazó hello tárfiók. |
| Azure SQL Database társított szolgáltatás |Az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. Hello másolási tevékenységhez hello kimeneti adatokat tartalmazó hello Azure SQL-adatbázist határoz meg. |
| Azure Blob bemeneti adatkészlet |Toohello Azure tárolás társított szolgáltatásának hivatkozik. hello társított szolgáltatás hivatkozik tooan Azure Storage-fiók, és határozza meg hello tároló, a mappa és a fájlnév hello Azure Blob-adathalmazra hello bemeneti adatokat tartalmazó hello tárolóban. |
| Az Azure SQL kimeneti adatkészlete |Toohello Azure SQL társított szolgáltatás hivatkozik. hello Azure SQL társított szolgáltatást hivatkozik tooan Azure SQL-kiszolgálót, és hello Azure SQL dataset hello kimeneti adatokat tartalmazó táblát hello hello nevét adja meg. |
| Adatfolyamat |hello folyamat rendelkezik egy tevékenységet, írja be, amely az Azure blob-adathalmazra hello bemenetként másolása és hello kimenetként Azure SQL-adatkészlet. hello másolási tevékenység hello Azure SQL Database az Azure blob tooa táblából másolja az adatokat. |

A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Kétféle típusú tevékenység létezik: az [adattovábbítási tevékenységek](data-factory-data-movement-activities.md) és az [adatátalakítási tevékenységek](data-factory-data-transformation-activities.md). Az oktatóanyag során létrehoz egy egyetlen tevékenységgel (másolási tevékenységgel) rendelkező folyamatot.

![Azure Blob tooAzure SQL-adatbázis másolása](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

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
Hozzon létre egy JSON fájlt **ADFCopyTutorialARM.json** a **C:\ADFGetStarted** hello tartalom a következő mappában:

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

## <a name="parameters-json"></a>Paramétereket tartalmazó JSON-file
Hozzon létre egy JSON fájlt **ADFCopyTutorialARM-Parameters.json** tartalmazó hello Azure Resource Manager sablon paramétereit. 

> [!IMPORTANT]
> Adja meg az Azure Storage-fiók nevét és kulcsát a storageAccountName és a storageAccountKey paraméterek értékeiként.  
> 
> Adja meg az Azure-beli SQL-kiszolgálót, az adatbázist, a felhasználót és a jelszót az sqlServerName, a databaseName, az sqlServerUserName és az sqlServerPassword paraméterek értékeiként.  

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
   * Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello.
    
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. Futtassa a következő parancs toodeploy adat-előállító entitások 1. lépésben létrehozott hello Resource Manager-sablon használatával hello.

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Folyamat figyelése

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) Azure-fiókjával.
2. Kattintson a **adat-előállítók** hello bal oldali menüben (vagy) kattintson **további szolgáltatások** kattintson **adat-előállítók** alatt **ESZKÖZINTELLIGENCIA + ANALITIKA** kategória.
   
    ![Adat-előállítók menü](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. A hello **adat-előállítók** lapon keresse meg és a data factory (AzureBlobToAzureSQLDatabaseDF) található. 
   
    ![Adat-előállító keresése](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Kattintson az Azure Data Factoryre. Az adat-előállító hello hello otthoni oldal akkor jelenik meg.
   
    ![Az adat-előállító kezdőlapja](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
6. Kövesse az utasításokat [adatkészletek és a folyamat figyelése](data-factory-copy-activity-tutorial-using-azure-portal.md#monitor-pipeline) toomonitor hello feldolgozási sorban lévő és adatkészletek ebben az oktatóanyagban létrehozott. A Visual Studio jelenleg nem támogatja a Data Factory-folyamatok monitorozását.
7. A szelet esetén hello **készen** állapot, győződjön meg arról, hogy hello adatok másolt toohello **üres** hello Azure SQL adatbázis táblájában.


Hogyan toouse az Azure portál paneleken toomonitor pipeline és adatkészletek létrehozott ebben az oktatóanyagban a további információkért lásd: [adatkészletek és a folyamat figyelése](data-factory-monitor-manage-pipelines.md) .

Hogyan toouse hello figyelő & kezelése alkalmazás toomonitor az adatok folyamatok további információkért lásd: [figyelése és kezelése az Azure Data Factory folyamatok figyelése alkalmazással](data-factory-monitor-manage-app.md).

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
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

Egy egyedi karakterlánc alapján hello erőforrás azonosítót.  

### <a name="defining-data-factory-entities"></a>Data Factory-entitások definiálása
hello következő adat-előállító entitások definiált hello JSON-sablon: 

1. [Azure Storage társított szolgáltatás](#azure-storage-linked-service)
2. [Azure SQL Database társított szolgáltatás](#azure-sql-database-linked-service)
3. [Azure Blob-adatkészlet](#azure-blob-dataset)
4. [Azure SQL-adatkészlet](#azure-sql-dataset)
5. [Másolási tevékenységgel rendelkező adatfolyamat](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
hello AzureStorageLinkedService művelet ugyan összeköti a az Azure storage-fiók toohello adat-előállítóban. Létrejött a tároló és részeként feltöltött adatok toothis tárfiók [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Ebben a szakaszban megadhatja hello nevét és az Azure storage-fiók kulcsát. Lásd: [Azure Storage társított szolgáltatásnak](data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak. 

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

hello connectionString hello storageAccountName és storageAccountKey paramétereket használja. Ezek a paraméterek az átadott konfigurációs fájl használatával hello értékeit. hello definition is változókat használ: hello sablonban meghatározott azureStroageLinkedService és dataFactoryName. 

#### <a name="azure-sql-database-linked-service"></a>Azure SQL Database társított szolgáltatás
AzureSqlLinkedService az Azure SQL adatbázis toohello adat-előállító hivatkozásokat tartalmaz. hello blobtárolóból másolt hello adatok az adatbázis tárolja. Hello üres tábla az adatbázisban részeként létrehozott [Előfeltételek](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md). Ebben a szakaszban megadhatja hello Azure SQL-kiszolgáló neve, az adatbázis nevét, a felhasználónév és a felhasználó jelszavát. Lásd: [Azure SQL társított szolgáltatásnak](data-factory-azure-sql-connector.md#linked-service-properties) JSON használt tulajdonságok toodefine vonatkozó további információért az Azure SQL társított szolgáltatásnak.  

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

hello connectionString használ sqlServerName, databaseName, sqlServerUserName és sqlServerPassword paraméterek, amelynek érték lett átadva a konfigurációs fájl segítségével. hello definition is használja következő változók hello sablonból hello: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Azure Blob-adatkészlet
hello Azure tárolás társított szolgáltatásának határozza meg a Data Factory szolgáltatásnak a futási idő tooconnect tooyour Azure storage-fiók használó hello kapcsolati karakterláncot. Az Azure blob adatkészlet-definícióban adja meg a blob-tároló, mappa, illetve hello bemeneti adatokat tartalmazó fájlt. Lásd: [Azure-Blob adatkészlet tulajdonságai](data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért. 

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

#### <a name="azure-sql-dataset"></a>Azure SQL-adatkészlet
Hello Azure SQL-adatbázis, amely a másolt hello adatait hello Azure Blob Storage tárolóban tárolja hello tábla hello nevét adja meg. Lásd: [Azure SQL adatkészlet tulajdonságai](data-factory-azure-sql-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure SQL-adatkészlet vonatkozó további információért. 

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

#### <a name="data-pipeline"></a>Adatfolyamat
Megadhatja egy folyamatot, amely adatokat másol hello Azure blob adatkészlet toohello Azure SQL-adatkészlet. Lásd: [adatcsatorna JSON](data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását. 

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

## <a name="reuse-hello-template"></a>Újbóli hello sablon
Hello oktatóanyagban létrehozott egy sablont a Data Factory entitásokat és a paraméterek értékeit, hogy a sablon meghatározásához. hello csővezeték adatok Azure Storage fiók tooan Azure SQL-adatbázis paraméterek keresztül másolja át. toouse hello egyező sablon toodeploy adat-előállító entitások toodifferent környezetekhez, hozzon létre egy paraméter környezetben, és használatra, ha toothat környezet telepítése.     

Példa:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

Figyelje meg, hogy az első parancs által használt paraméter hello hello fejlesztői környezetben hello tesztkörnyezetben, a másikat a fájlt, és hello harmadik egy hello éles környezetben.  

Hello sablon tooperform is felhasználhatja ismétlődő feladatokat. Például egy sok adat-előállítók kell toocreate, vagy további folyamatok, amelyek megvalósítják az azonos hello logikát, de minden adat-előállító külön tárolási és SQL-adatbázis fiókot használja. Ebben a forgatókönyvben hello használhatja ugyanazt a sablont a hello ugyanabban a környezetben (fejlesztői, tesztelési vagy éles) különböző paraméterrel toocreate adat-előállítók fájlok.   

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag egy olyan másolási műveletet mutatott be, amelynek a forrásadattára egy Azure Blob Storage-tár, a céladattára pedig egy Azure SQL-adatbázis volt. hello következő táblázat felsorolja az adatforrások és a célhelyek hello másolási tevékenység által támogatott adattárolókhoz: 

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Hogyan toocopy adatok belőle egy adatok tárolására, kapcsolatos toolearn hivatkozásra hello hello adattároló hello tábla.
