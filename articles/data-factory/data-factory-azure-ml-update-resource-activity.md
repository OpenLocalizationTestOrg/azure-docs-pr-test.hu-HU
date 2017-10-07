---
title: "Azure Data Factory használatával aaaUpdate gépi tanulási modellek |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 6e5e4d2cfd245c7a9ed3bb9cdacca1f7f82b9620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="updating-azure-machine-learning-models-using-update-resource-activity"></a>Frissítés Azure Machine Learning modellek használata az Update-Erőforrástevékenység

> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-tevékenység](data-factory-hive-activity.md) 
> * [A Pig-tevékenység](data-factory-pig-activity.md)
> * [MapReduce művelethez](data-factory-map-reduce.md)
> * [Hadoop Streamelési tevékenységben](data-factory-hadoop-streaming-activity.md)
> * [A Spark-tevékenység](data-factory-spark.md)
> * [Machine Learning kötegelt végrehajtási tevékenység](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning Update-erőforrástevékenység](data-factory-azure-ml-update-resource-activity.md)
> * [Tárolt eljárási tevékenység](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-tevékenység](data-factory-usql-activity.md)
> * [.NET egyéni tevékenység](data-factory-use-custom-activities.md)

Ez a cikk kiegészítés hello fő Azure Data Factory - Azure Machine Learning integrációs cikk: [létrehozása az Azure Machine Learning és az Azure Data Factory használatával prediktív folyamatok](data-factory-azure-ml-batch-execution-activity.md). Ha még nem tette meg, tekintse át a cikk fő hello keresztül ez a cikk elolvasása előtt. 

## <a name="overview"></a>Áttekintés
Az idő múlásával hello prediktív modelleket hello Azure ML pontozási kísérletekben kell toobe retrained új bemeneti adatkészletek használata. Miután elkészült, az átképezési, kívánt hello webszolgáltatás pontozási tooupdate hello retrained ML modellje. hello jellemzően előforduló lépéseket tooenable átképezési és webszolgáltatásokkal frissítése Azure ML-modellek a következők:

1. A kísérlet létrehozásának [Azure ML Studio](https://studio.azureml.net).
2. Ha elégedett hello modell, használja az Azure ML Studio toopublish webszolgáltatás-mindkét hello **tanítási kísérletet** és pontozási /**prediktív kísérletté**.

hello következő táblázat ismerteti a példában szereplő hello webszolgáltatások.  Lásd: [Machine Learning-modellek szoftveres](../machine-learning/machine-learning-retrain-models-programmatically.md) részleteiről.

- **Webszolgáltatás betanítása** - tanítási adatokat fogad, és hozza létre a betanított modellek. hello hello átképezési eredménye egy .ilearner fájlt az Azure Blob Storage tárolóban. Hello **alapértelmezett végpont** automatikusan létrejön a hello képzési közzétételekor kísérletezik webszolgáltatásként. Létrehozhat további végpontok, de hello példában csak a hello alapértelmezett végpont.
- **Webszolgáltatás pontozási** – címke nélküli adatok példák kap, és lehetővé teszi az előrejelzés. hello kimeneti előrejelzési lehet különböző formát, például egy CSV-fájlt vagy egy Azure SQL-adatbázis hello kísérlet hello konfigurációjától függően sorokat. hello alapértelmezett végpont az Ön automatikusan jön létre, hello prediktív kísérletté webszolgáltatásként közzétételekor. 

hello alábbi kép ábrázolja képzési és a végpontok pontozás az Azure ml hello kapcsolatát.

![Webszolgáltatások](./media/data-factory-azure-ml-batch-execution-activity/web-services.png)

Hello hívhat meg **webszolgáltatás betanítása** hello segítségével **Azure ML kötegelt végrehajtási tevékenység**. Egy képzési webszolgáltatás indítására legyen, mint az Azure gépi tanulás webszolgáltatás (pontozási webszolgáltatás) való pontozási adatokat. hello hogyan tooinvoke az Azure gépi tanulás webszolgáltatás egy Azure Data Factory a csővezeték-részletesen előző szakaszokban lefedi. 

Hello hívhat meg **webszolgáltatás pontozási** hello segítségével **Azure ML Update-Erőforrástevékenység** tooupdate hello webszolgáltatás hello újonnan betanított modell. a következő példák hello társított szolgáltatás definíciókat tartalmazza: 

## <a name="scoring-web-service-is-a-classic-web-service"></a>Webszolgáltatás pontozási egy klasszikus webszolgáltatás-bővítmény
Webszolgáltatás pontozási hello esetén egy **klasszikus webszolgáltatás**, hozzon létre második hello **nem alapértelmezett és frissíthető végpont** hello segítségével [Azure-portálon](https://manage.windowsazure.com). Lásd: [végpontok létrehozása](../machine-learning/machine-learning-create-endpoint.md) cikk lépéseit. Miután létrehozta a hello nem alapértelmezett frissíthető végpont, hello a következő lépéseket:

* Kattintson a **KÖTEGELT végrehajtási** tooget hello URI érték hello **mlEndpoint** JSON tulajdonság.
* Kattintson a **frissítés erőforrás** tooget hello URI értéke hello hivatkozás **updateResourceEndpoint** JSON tulajdonság. hello API-kulcs van oldalon hello végpont magát (a hello jobb alsó sarokban).

![frissíthető végpont](./media/data-factory-azure-ml-batch-execution-activity/updatable-endpoint.png)

a következő példa hello biztosít egy minta hello AzureML társított szolgáltatás JSON definíciója. hello társított szolgáltatás hello apiKey, használ.  

```json
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--scoring experiment--/jobs",
            "apiKey": "endpoint2Key",
            "updateResourceEndpoint": "https://management.azureml.net/workspaces/xxx/webservices/--scoring experiment--/endpoints/endpoint2"
        }
    }
}
```

## <a name="scoring-web-service-is-azure-resource-manager-web-service"></a>Webszolgáltatás pontozás az Azure Resource Manager webszolgáltatás 
Ha hello webszolgáltatás hello új webes szolgáltatás, amely elérhetővé teszi az Azure Resource Manager-végpont típusú, nem kell tooadd hello második **nem alapértelmezett** végpont. Hello **updateResourceEndpoint** hello a társított szolgáltatás hello formátum van: 

```
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resource-group-name}/providers/Microsoft.MachineLearning/webServices/{web-service-name}?api-version=2016-05-01-preview. 
```

Kaphat értékek hely tulajdonosainak hello URL-címben hello hello webhellyel lekérdezésekor [Azure Machine Learning Web Services portálra](https://services.azureml.net/). frissítés erőforrás végpont új típusú hello igényel az (Azure Active Directory) AAD-tokent. Adja meg **servicePrincipalId** és **servicePrincipalKey**az AzureML társított szolgáltatás. Lásd: [hogyan toocreate egyszerű szolgáltatást, és rendelje hozzá az engedélyek toomanage Azure-erőforrás](../azure-resource-manager/resource-group-create-service-principal-portal.md). Íme egy minta AzureML társított szolgáltatás definíciójának: 

```json
{
    "name": "AzureMLLinkedService",
    "properties": {
        "type": "AzureML",
        "description": "hello linked service for AML web service.",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/0000000000000000000000000000000000000/services/0000000000000000000000000000000000000/jobs?api-version=2.0",
            "apiKey": "xxxxxxxxxxxx",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myRG/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "000000000-0000-0000-0000-0000000000000",
            "servicePrincipalKey": "xxxxx",
            "tenant": "mycompany.com"
        }
    }
}
```

hello következő forgatókönyv további részleteket tartalmaz. Rendelkezik egy példa átképezési, és az Azure Data Factory-folyamat az Azure ML modellek frissítéséhez.

## <a name="scenario-retraining-and-updating-an-azure-ml-model"></a>Forgatókönyv: átképezési, és az Azure ML modellje frissítése
Ez a témakör egy minta folyamat által használt hello **Azure ML kötegelt végrehajtási tevékenység** tooretrain egy modell. hello folyamatot is alkalmaz hello **Azure ML Update erőforrástevékenység** tooupdate hello modell pontozása webszolgáltatás hello. hello szakasz is biztosít JSON kódtöredékek összes hello összekapcsolt szolgáltatások, az adatkészleteket és a kimenetátirányítási mechanizmusával hello példa.

Itt található hello diagram nézet hello minta folyamatának. Ahogy látja, hello Azure ML kötegelt végrehajtási tevékenység hello képzési bemenetből fogad adatokat, és a képzési kimenetet (iLearner-fájlt). hello Azure ML Update-Erőforrástevékenység időt vesz igénybe, a képzési kimenetet, és a frissítések hello webszolgáltatási végpontot pontozási hello modellt. hello Update-Erőforrástevékenység nem ad kimenetet. hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.

![folyamat diagramja](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

### <a name="azure-blob-storage-linked-service"></a>Az Azure Blob storage társított szolgáltatásnak:
hello Azure Storage hello a következő adatokat tartalmazza:

* betanítási adata. hello bemeneti adatok hello Azure ML képzési webszolgáltatáshoz.  
* iLearner-fájlt. hello hello Azure ML képzési webszolgáltatás kimenetét. A fájl is hello bemeneti toohello Update-erőforrástevékenység.  

Hello minta JSON-definícióból kapcsolódó hello szolgáltatást a következő:

```JSON
{
    "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=name;AccountKey=key"
        }
    }
}
```

### <a name="training-input-dataset"></a>Képzési bemeneti adatkészlet:
hello következő dataset hello bemeneti tanítási adatokat képvisel hello Azure ML képzési webszolgáltatáshoz. Azure ML kötegelt végrehajtási tevékenység hello ehhez az adatkészlethez bemenetként vesz igénybe.

```JSON
{
    "name": "trainingData",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "labeledexamples",
            "fileName": "labeledexamples.arff",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
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

### <a name="training-output-dataset"></a>Képzési kimeneti adatkészlet:
hello következő dataset jelöli hello kimeneti iLearner-fájlt hello Azure ML képzési webszolgáltatáshoz. hello Azure ML kötegelt végrehajtási tevékenység létrehozza az adatkészletet. Ez az adatkészlet esetében is hello bemeneti toohello Azure ML Update erőforrás tevékenység.

```JSON
{
    "name": "trainedModelBlob",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "trainingoutput",
            "fileName": "model.ilearner",
            "format": {
                "type": "TextFormat"
            }
        },
        "availability": {
            "frequency": "Week",
            "interval": 1
        }
    }
}
```

### <a name="linked-service-for-azure-ml-training-endpoint"></a>Azure ML képzési végponthoz társított szolgáltatás
a következő JSON részlet hello határozza meg az Azure Machine Learning társított szolgáltatást az toohello alapértelmezett végpont hello képzési webszolgáltatás mutat.

```JSON
{    
    "name": "trainingEndpoint",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/xxx/services/--training experiment--/jobs",
              "apiKey": "myKey"
        }
      }
}
```

A **Azure ML Studio**, tooget értékeket a következő hello **mlEndpoint** és **apiKey**:

1. Kattintson a **WEBSZOLGÁLTATÁSOK** hello bal oldali menüben.
2. Kattintson a hello **webszolgáltatás betanítása** webszolgáltatások hello listájában.
3. Kattintson a Másolás tovább túl**API-kulcs** szövegmezőben. Illessze be hello kulcs hello vágólapon hello Data Factory JSON-szerkesztőt.
4. A hello **Azure ML studio**, kattintson a **KÖTEGELT végrehajtási** hivatkozásra.
5. Másolás hello **kérelem URI-azonosítója** a hello **kérelem** szakaszt, és illessze be hello Data Factory JSON-szerkesztőt.   

### <a name="linked-service-for-azure-ml-updatable-scoring-endpoint"></a>Azure ML frissíthető pontozási végponthoz társított szolgáltatás:
a következő JSON részlet hello határozza meg az Azure Machine Learning társított szolgáltatást az toohello nem alapértelmezett frissíthető végpontja webszolgáltatás pontozási hello mutat.  

```JSON
{
    "name": "updatableScoringEndpoint2",
    "properties": {
        "type": "AzureML",
        "typeProperties": {
            "mlEndpoint": "https://ussouthcentral.services.azureml.net/workspaces/00000000eb0abe4d6bbb1d7886062747d7/services/00000000026734a5889e02fbb1f65cefd/jobs?api-version=2.0",
            "apiKey": "sooooooooooh3WvG1hBfKS2BNNcfwSO7hhY6dY98noLfOdqQydYDIXyf2KoIaN3JpALu/AKtflHWMOCuicm/Q==",
            "updateResourceEndpoint": "https://management.azure.com/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/myWebService?api-version=2016-05-01-preview",
            "servicePrincipalId": "fe200044-c008-4008-a005-94000000731",
            "servicePrincipalKey": "zWa0000000000Tp6FjtZOspK/WMA2tQ08c8U+gZRBlw=",
            "tenant": "mycompany.com"
        }
    }
}
```

### <a name="placeholder-output-dataset"></a>Helyőrző kimeneti adatkészlet:
hello Azure ML Update erőforrás tevékenység nem ad kimenetet. Azure Data Factory azonban egy kimeneti adatkészlet toodrive hello ütemezést adatcsatorna igényel. A helyőrző/helyőrző dataset ezért ebben a példában használjuk.  

```JSON
{
    "name": "placeholderBlob",
    "properties": {
        "availability": {
            "frequency": "Week",
            "interval": 1
        },
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "any",
            "format": {
                "type": "TextFormat"
            }
        }
    }
}
```

### <a name="pipeline"></a>Folyamat
hello folyamat két tevékenység rendelkezik: **AzureMLBatchExecution** és **AzureMLUpdateResource**. hello Azure ML kötegelt végrehajtási tevékenység hello betanítási adatok bemenetként vesz igénybe, és egy kimenetként iLearner-fájlt hoz létre. hello tevékenység hello képzési webszolgáltatás (a tanítási kísérletet webszolgáltatásként kitett) hív hello bevitellel betanítási adatok, és hello ilearner-fájlt kapott hello webszolgáltatás. hello placeholderBlob csak hello Azure Data Factory szolgáltatás toorun hello-folyamat által igényelt üres kimeneti adatkészlet.

![folyamat diagramja](./media/data-factory-azure-ml-batch-execution-activity/update-activity-pipeline-diagram.png)

```JSON
{
    "name": "pipeline",
    "properties": {
        "activities": [
            {
                "name": "retraining",
                "type": "AzureMLBatchExecution",
                "inputs": [
                    {
                        "name": "trainingData"
                    }
                ],
                "outputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "typeProperties": {
                    "webServiceInput": "trainingData",
                    "webServiceOutputs": {
                        "output1": "trainedModelBlob"
                    }              
                 },
                "linkedServiceName": "trainingEndpoint",
                "policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            },
            {
                "type": "AzureMLUpdateResource",
                "typeProperties": {
                    "trainedModelName": "Training Exp for ADF ML [trained model]",
                    "trainedModelDatasetName" :  "trainedModelBlob"
                },
                "inputs": [
                    {
                        "name": "trainedModelBlob"
                    }
                ],
                "outputs": [
                    {
                        "name": "placeholderBlob"
                    }
                ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "AzureML Update Resource",
                "linkedServiceName": "updatableScoringEndpoint2"
            }
        ],
        "start": "2016-02-13T00:00:00Z",
           "end": "2016-02-14T00:00:00Z"
    }
}
```
