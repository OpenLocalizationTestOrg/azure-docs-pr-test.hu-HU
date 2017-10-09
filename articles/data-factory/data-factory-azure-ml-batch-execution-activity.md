---
title: "Azure Data Factory használatával aaaCreate prediktív adatok folyamatok |} Microsoft Docs"
description: "Ismerteti, hogyan toocreate létre Azure Data Factory és az Azure Machine Learning a prediktív folyamatok"
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 4fad8445-4e96-4ce0-aa23-9b88e5ec1965
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 943210c28b1696e299ff9b7cc96369b95f182354
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-predictive-pipelines-using-azure-machine-learning-and-azure-data-factory"></a>Hozzon létre prediktív folyamatok Azure Machine Learning és az Azure Data Factory használatával

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

## <a name="introduction"></a>Bevezetés

### <a name="azure-machine-learning"></a>Azure Machine Learning
[Az Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) lehetővé teszi, hogy toobuild, tesztelését és rendszerbe prediktív elemzési megoldások. Magas szintű szempontból elkészült a három lépést:

1. **Hozzon létre egy tanítási kísérletet**. Ez a lépés hello Azure ML Studio úgy teheti meg. hello ML studio olyan együttműködési Látványelem-fejlesztési környezet tootrain használni, és tesztelje a prediktív elemzési modell betanítási adatok használatával.
2. **Alakítsa át a tooa prediktív kísérletté**. Ha a modell még betanítva meglévő adatokkal, és készen áll a toouse azt tooscore új adatokat, előkészítéséhez és pontozó kísérletbe egyszerűsítésére.
3. **A webszolgáltatás üzembe**. A pontozási kísérlet közzéteheti Azure webszolgáltatásként. Is tooyour adatmodell keresztül a webes szolgáltatás végpontját küldése és fogadása eredmény előrejelzéseket eredete hello modell.  

### <a name="azure-data-factory"></a>Azure Data Factory
Adat-előállító koordinálja és automatizálja a hello felhőalapú adatok integrációs szolgáltatás **adatátviteli** és **átalakítása** adatok. Adatok integrációs megoldásokat Azure Data Factory használatával is különböző adattárolókhoz származó adatok, átalakító/folyamat hello adatok és közzététele hello eredmény adatok toohello adattárolókhoz hozhat létre.

Data Factory szolgáltatásnak lehetővé teszi, hogy helyezze át, és az adatok átalakítása toocreate adatok folyamatok, és futtassa a hello folyamatok meghatározott ütemezés szerint (óránként, naponta, hetente, stb.). Emellett biztosít gazdag képi megjelenítések toodisplay hello Leszármaztatás és az adatok folyamatok közti függőségeket és minden az adatok folyamatok egy egyetlen egyesítve láthassák tooeasily meghatározhatja problémák figyelése és beállítása a figyelési riasztásokat.

Lásd: [Data Factory bemutatása tooAzure](data-factory-introduction.md) és [felépítheti első folyamatát](data-factory-build-your-first-pipeline.md) cikkek tooquickly hello Azure Data Factory szolgáltatásnak az első lépései.

### <a name="data-factory-and-machine-learning-together"></a>Adat-előállító és a gépi tanulás együtt
Az Azure Data Factory lehetővé teszi, hogy Ön tooeasily hozzon létre egy közzétett használó folyamatok [Azure Machine Learning] [ azure-machine-learning] webszolgáltatás prediktív elemzéséhez. Hello segítségével **kötegelt végrehajtási tevékenység** egy Azure Data Factory-folyamat a hívhat meg az Azure ML web toomake előrejelzések kötegben hello adatokon. Lásd: [meghívása az Azure ML kötegelt végrehajtási tevékenység webes szolgáltatás használatával hello](#invoking-an-azure-ml-web-service-using-the-batch-execution-activity) című szakaszban talál információt.

Az idő múlásával hello prediktív modelleket hello Azure ML pontozási kísérletekben kell toobe retrained új bemeneti adatkészletek használata. A Data Factory-folyamat az Azure ML modellje is újratanítása hello lépések végrehajtásával:

1. Tegye közzé a hello tanítási kísérletet (nem prediktív kísérletté) webszolgáltatásként. Ezt megteheti ezt a lépést, az Azure ML Studio hello úgy, ahogy tooexpose prediktív kísérletté hello előző forgatókönyvben webszolgáltatásként.
2. Hello Azure ML kötegelt végrehajtási tevékenység tooinvoke hello webes szolgáltatás hello tanítási kísérletet segítségével. Alapvetően hello Azure ML kötegelt végrehajtási tevékenység tooinvoke webszolgáltatás képzési és a webszolgáltatás pontozási is használhatja.

Után átképezési befejezése, frissítse a webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) pontozási hello újonnan betanított modell hello segítségével hello **Azure ML Update Erőforrástevékenység**. Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.

## <a name="invoking-a-web-service-using-batch-execution-activity"></a>Meghívja a webszolgáltatást kötegelt végrehajtási tevékenység
Azure Data Factory tooorchestrate adatmozgatást és feldolgozási használja, és hajtsa végre az Azure Machine Learning kötegelt végrehajtását. Az alábbiakban hello legfelső szintű lépéseket:

1. Hozzon létre egy Azure Machine Learning társított szolgáltatást. A következő értékek hello lesz szüksége:

   1. **A kérelmi URI** hello kötegelt végrehajtási API számára. Hello kérelem URI-CÍMÉN található hello kattintva **KÖTEGELT végrehajtási** hello szolgáltatások weblap hivatkozásra.
   2. **Az API-kulcs** a hello közzétett Azure Machine Learning webszolgáltatáshoz. Hello API-kulcs hello webszolgáltatás, amelyet a közzétett kattintva találja.
   3. Használjon hello **AzureMLBatchExecution** tevékenység.

      ![Machine Learning irányítópult](./media/data-factory-azure-ml-batch-execution-activity/AzureMLDashboard.png)

      ![Kötegelt URI](./media/data-factory-azure-ml-batch-execution-activity/batch-uri.png)

### <a name="scenario-experiments-using-web-service-inputsoutputs-that-refer-toodata-in-azure-blob-storage"></a>Forgatókönyv: Kísérletek webes szolgáltatás bemenetek/kimenetek, tekintse meg az Azure Blob Storage toodata használatával
Ebben a forgatókönyvben a hello Azure Machine Learning webszolgáltatás lehetővé teszi az adatok segítségével az Azure blob Storage-fájlból való előrejelzéseket, és hello előrejelzés eredmények hello blob Storage tárolóban tárolja. hello következő JSON határozza meg a Data Factory-folyamathoz egy AzureMLBatchExecution tevékenységet. hello tevékenység rendelkezik hello dataset **DecisionTreeInputBlob** bemenetként és **DecisionTreeResultBlob** hello output típusúként. Hello **DecisionTreeInputBlob** átadása egy bemeneti toohello webszolgáltatásként által hello segítségével **típus** JSON tulajdonság. Hello **DecisionTreeResultBlob** átadása egy kimeneti toohello webszolgáltatás által hello segítségével **webServiceOutputs** JSON tulajdonság.  

> [!IMPORTANT]
> Ha hello webszolgáltatás több bemeneti vesz igénybe, használja a hello **webServiceInputs** tulajdonság használata helyett **típus**. Lásd: hello [webszolgáltatás több bemeneti értéket igényel](#web-service-requires-multiple-inputs) szakasz egy példát hello webServiceInputs tulajdonság használatával.
>
> Hello által hivatkozott adatkészletek **típus**/**webServiceInputs** és **webServiceOutputs** tulajdonságok (a  **typeProperties**) is szerepelnie kell hello tevékenység **bemenetek** és **kimenete**.
>
> Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható. webServiceInputs, webServiceOutputs és globalParameters beállítások használt hello neveket hello kísérletekben hello neveket pontosan egyeznie kell. Az Azure ML végpont tooverify várt hello leképezés hello minta-kérések forgalma hello kötegelt végrehajtási súgó lapon tekintheti meg.
>
>

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchExecution",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "DecisionTreeInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "DecisionTreeResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "typeProperties":
        {
            "webServiceInput": "DecisionTreeInputBlob",
            "webServiceOutputs": {
                "output1": "DecisionTreeResultBlob"
            }                
        },
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```
> [!NOTE]
> Csak be- és kimenetekkel hello AzureMLBatchExecution tevékenység paraméterek toohello webszolgáltatás argumentumként átadhatók. A fenti JSON részlet hello, például a DecisionTreeInputBlob egy bemeneti toohello AzureMLBatchExecution tevékenység, mint egy bemeneti toohello webszolgáltatás átadott típus paraméteren keresztül.   
>
>

### <a name="example"></a>Példa
A példa használja az Azure Storage toohold mindkét hello bemeneti és kimeneti adatokat.

Javasoljuk, hogy olvassa végig hello [felépítheti első folyamatát Data Factory] [ adf-build-1st-pipeline] oktatóanyag, mielőtt továbblépne ebben a példában keresztül. Ebben a példában hello Data Factory Editor toocreate adat-előállító összetevők (társított szolgáltatások, adatkészleteket, pipeline) használja.   

1. Hozzon létre egy **társított szolgáltatás** a a **Azure Storage**. Ha hello bemeneti és kimeneti fájlok eltérő tárfiókokból, két összekapcsolt szolgáltatások szüksége. Íme egy JSON-példa:

    ```JSON
    {
      "name": "StorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=[acctName];AccountKey=[acctKey]"
        }
      }
    }
    ```
2. Hozzon létre hello **bemeneti** Azure Data Factory **dataset**. Néhány egyéb adat-előállító adatkészletek ellentétben ezek az adatkészletek tartalmaznia kell mindkét **folderPath** és **Fájlnév** értékeket. Használhat particionálási toocause minden kötegelt végrehajtási (minden adatszelet) tooprocess vagy tud létrehozni egyedi bemeneti és kimeneti fájlok. Szükség lehet néhány felsőbb szintű tevékenység tootransform hello hello CSV-fájlformátumot bemeneteként, és helyezheti el hello tárfiókot az egyes szeletek tooinclude. Ebben az esetben nem tartalmazná hello **külső** és **externalData** látható példa, és a DecisionTreeInputBlob lenne hello kimeneti adatkészlet egy másik tevékenység a következő hello beállításait.

    ```JSON
    {
      "name": "DecisionTreeInputBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/input",
          "fileName": "in.csv",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "external": true,
        "availability": {
          "frequency": "Day",
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

    A bemeneti csv-fájlban hello oszlop fejlécsor kell rendelkeznie. Hello használata **másolási tevékenység** toocreate/move hello csv hello blob tárolóba, célszerű hello fogadó tulajdonság **blobWriterAddHeader** túl**igaz**. Példa:

    ```JSON
    sink:
    {
        "type": "BlobSink",     
        "blobWriterAddHeader": true
    }
    ```

    Hello csv-fájl nem rendelkezik hello fejlécsor, jelenhet meg a következő hiba hello: **hiba a tevékenység: Hiba történt a karakterlánc olvasásakor. Váratlan lexikális elem: StartObject. Elérési út ", 1. sor, 1 elhelyezése**.
3. Hozzon létre hello **kimeneti** Azure Data Factory **dataset**. A példa particionálási toocreate egy egyedi kimeneti elérési út minden szelet végrehajtásra. Hello particionálás, nélkül hello tevékenység felülírná hello fájlt.

    ```JSON
    {
      "name": "DecisionTreeResultBlob",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "azuremltesting/scored/{folderpart}/",
          "fileName": "{filepart}result.csv",
          "partitionedBy": [
            {
              "name": "folderpart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "yyyyMMdd"
              }
            },
            {
              "name": "filepart",
              "value": {
                "type": "DateTime",
                "date": "SliceStart",
                "format": "HHmmss"
              }
            }
          ],
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ","
          }
        },
        "availability": {
          "frequency": "Day",
          "interval": 15
        }
      }
    }
    ```
4. Hozzon létre egy **társított szolgáltatás** típusú: **AzureMLLinkedService**, hello API-kulcsot biztosító és modellhez tartozó kötegelt végrehajtás URL-CÍMÉT.

    ```JSON
    {
      "name": "MyAzureMLLinkedService",
      "properties": {
        "type": "AzureML",
        "typeProperties": {
          "mlEndpoint": "https://[batch execution endpoint]/jobs",
          "apiKey": "[apikey]"
        }
      }
    }
    ```
5. Végezetül szerzői a folyamat, amely tartalmazza egy **AzureMLBatchExecution** tevékenység. Futásidőben a folyamat hello a lépéseket követve hajtja végre:

   1. A bemeneti adatkészletek hello bemeneti fájl helye hello lekérése.
   2. Meghívja a hello Azure Machine Learning kötegelt végrehajtási API
   3. Másolja a kötegelt végrehajtási kimeneti toohello blob a kimeneti adatkészlet megadott hello.

      > [!NOTE]
      > AzureMLBatchExecution tevékenység állhat nulla vagy több be- és egy vagy több kimenetekkel.
      >
      >

    ```JSON
    {
        "name": "PredictivePipeline",
        "properties": {
            "description": "use AzureML model",
            "activities": [
            {
                "name": "MLActivity",
                "type": "AzureMLBatchExecution",
                "description": "prediction analysis on batch input",
                "inputs": [
                {
                    "name": "DecisionTreeInputBlob"
                }
                ],
                "outputs": [
                {
                    "name": "DecisionTreeResultBlob"
                }
                ],
                "linkedServiceName": "MyAzureMLLinkedService",
                "typeProperties":
                {
                    "webServiceInput": "DecisionTreeInputBlob",
                    "webServiceOutputs": {
                        "output1": "DecisionTreeResultBlob"
                    }                
                },
                "policy": {
                    "concurrency": 3,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 1,
                    "timeout": "02:00:00"
                }
            }
            ],
            "start": "2016-02-13T00:00:00Z",
            "end": "2016-02-14T00:00:00Z"
        }
    }
    ```

      Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601). Például: 2014-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező. Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**" toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság. A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).

      > [!NOTE]
      > A megadott hello AzureMLBatchExecution tevékenység megadása nem kötelező megadni.
      >
      >

### <a name="scenario-experiments-using-readerwriter-modules-toorefer-toodata-in-various-storages"></a>Forgatókönyv: Kísérletek olvasási/írási modulok toorefer toodata különböző tárolók használata
Egy másik forgatókönyve az Azure ML kísérletek létrehozásakor toouse írási és olvasási szerepkörökhöz modulok. hello olvasó modul kísérlet használt tooload adatokat, és hello író modul toosave adatok a kísérletekből. Írási és olvasási szerepkörökhöz modullal kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában.     

Hello írási és olvasási szerepkörökhöz modulok használata esetén célszerű toouse olvasási/írási modulok minden egyes tulajdonság egy webszolgáltatási paraméter is. Ezek a paraméterek lehetővé teszik a tooconfigure hello értékek futásidőben. Például egy olvasó modul, amely használja az Azure SQL Database segítségével létrehozhat egy kísérlet: XXX.database.windows.net. Hello webszolgáltatás telepítése után szeretné tooenable hello fogyasztói hello web service toospecify egy másik Azure SQL Server YYY.database.windows.net hívása. Egy webes szolgáltatás paraméter tooallow a konfigurált érték toobe használható.

> [!NOTE]
> Webes szolgáltatás bemeneti és kimeneti eltérnek a webszolgáltatás-paramétereket. Hello első forgatókönyvben láthatta, hogyan egy bemeneti és kimeneti adható meg az Azure gépi tanulás webszolgáltatás. Ebben a forgatókönyvben át egy webszolgáltatás, amelyek megfelelnek az olvasási/írási modulok tooproperties paramétereit.
>
>

Vizsgáljuk meg egy olyan helyzetben használhat webszolgáltatás-paramétereket. Egy telepített Azure Machine Learning webszolgáltatás, amelyet egy olvasó modul tooread adatokat használ az Azure Machine Learning által támogatott adatforrások hello egyik rendelkezik (például: Azure SQL Database). Hello kötegelt végrehajtási hajtja végre, hello eredmények írt író modul (az Azure SQL Database) használatával.  Nincs a webszolgáltatás bemenetei és kimenetei hello kísérletek vannak definiálva. Ebben az esetben ajánlott, hogy konfigurálja a hello írási és olvasási szerepkörökhöz modulok vonatkozó webszolgáltatás-paramétereket. Ez a konfiguráció lehetővé teszi, hogy hello olvasási/írási modulok toobe az hello AzureMLBatchExecution tevékenység konfigurálva. Webszolgáltatás-paramétereket ad meg hello **globalParameters** hello tevékenység JSON szakasz az alábbiak szerint.

```JSON
"typeProperties": {
    "globalParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```

Is [Data Factory funkciók](data-factory-functions-variables.md) a sikeres hello értékeinek webszolgáltatás-paramétereket a hello a következő példában látható módon:

```JSON
"typeProperties": {
    "globalParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Webszolgáltatás-paramétereket hello kis-és nagybetűket, ezért figyeljen oda arra, hogy hello hello tevékenységben megadott JSON megfelel hello hello webszolgáltatás által elérhetővé tett néhányat a meglévők közül.
>
>

### <a name="using-a-reader-module-tooread-data-from-multiple-files-in-azure-blob"></a>Az Azure Blob több fájlból egy olvasó modul tooread adatok használata
Big Data típusú adatok folyamatok a tevékenységeket, például a Pig és Hive is létrehozhat egy vagy több kimeneti fájlokat a bővítmények nincsenek. Például egy külső Hive tábla megadásakor hello külső Hive tábla adatai hello tárolhatók az Azure blob storage a következő neve 000000_0 hello. Egy kísérletben tooread hello Adatolvasó modulja több fájlt használja, és előrejelzéseket használhatja őket.

Az Azure Machine Learning kísérletben hello Adatolvasó modulja használatakor a Azure Blob bemenetként is megadhat. az Azure blob storage hello hello fájlok lehetnek a hello kimeneti fájlok (Példa: 000000_0), amely a HDInsight-on futó Pig és a Hive parancsfájlok hozzák létre. hello olvasó modul lehetővé teszi tooread fájlok (nincs) hello konfigurálásával **elérési toocontainer, könyvtár vagy blob**. Hello **elérési toocontainer** pontok toohello tároló és **könyvtár/blob** toofolder, ahogy az a következő kép hello hello fájlokat tartalmazó mutat. hello csillag, ez azt jelenti, hogy \*) **határozza meg, hogy az összes hello hello tároló/mappában található fájlokat (Ez azt jelenti, hogy adatokat/aggregateddata/év 2014/hónap-6 = /\*)** hello kísérlet részeként olvasható.

![Az Azure Blob tulajdonságai](./media/data-factory-create-predictive-pipelines/azure-blob-properties.png)

### <a name="example"></a>Példa
#### <a name="pipeline-with-azuremlbatchexecution-activity-with-web-service-parameters"></a>A webszolgáltatás-paramétereket AzureMLBatchExecution tevékenység-feldolgozási folyamat

```JSON
{
  "name": "MLWithSqlReaderSqlWriter",
  "properties": {
    "description": "Azure ML model with sql azure reader/writer",
    "activities": [
      {
        "name": "MLSqlReaderSqlWriterActivity",
        "type": "AzureMLBatchExecution",
        "description": "test",
        "inputs": [
          {
            "name": "MLSqlInput"
          }
        ],
        "outputs": [
          {
            "name": "MLSqlOutput"
          }
        ],
        "linkedServiceName": "MLSqlReaderSqlWriterDecisionTreeModel",
        "typeProperties":
        {
            "webServiceInput": "MLSqlInput",
            "webServiceOutputs": {
                "output1": "MLSqlOutput"
            }
              "globalParameters": {
                "Database server name": "<myserver>.database.windows.net",
                "Database name": "<database>",
                "Server user account name": "<user name>",
                "Server user account password": "<password>"
              }              
        },
        "policy": {
          "concurrency": 1,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        },
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

A fenti JSON példa hello:

* hello telepített Azure Machine Learning Web szolgáltatás használja az olvasót, és egy író modul tooread/adatok írása az / tooan Azure SQL Database. Ez a webszolgáltatás mutatja meg a következő négy paraméterek hello: adatbázis-kiszolgáló neve, a adatbázis neve, a kiszolgáló felhasználói fiók nevét és a kiszolgáló felhasználói fiók jelszavát.  
* Mindkét **start** és **end** időpontok szerepelnie kell [ISO formátum](http://en.wikipedia.org/wiki/ISO_8601). Például: 2014-10-14T16:32:41Z. Hello **end** idő megadása nem kötelező. Ha nem ad meg értéket a hello **end** tulajdonságot, akkor a program "**kezdés + 48 óra.**" toorun hello folyamat határozatlan ideig, adja meg **9999-09-09** hello hello értékként **end** tulajdonság. A JSON-tulajdonságokkal kapcsolatos információkért lásd: [JSON Scripting Reference](https://msdn.microsoft.com/library/dn835050.aspx) (Referencia a JSON-parancsprogramokhoz).

### <a name="other-scenarios"></a>Egyéb forgatókönyvek
#### <a name="web-service-requires-multiple-inputs"></a>Webszolgáltatás több bemeneti értéket igényel
Ha hello webszolgáltatás több bemeneti vesz igénybe, használja a hello **webServiceInputs** tulajdonság használata helyett **típus**. Hello által hivatkozott adatkészletek **webServiceInputs** is szerepelnie kell hello tevékenység **bemenetek**.

Az Azure ML kísérletben webszolgáltatás bemenetét és a kimeneti portok és a globális paraméterek alapértelmezett neve lehet ("input1", "input2"), amely testre szabható. webServiceInputs, webServiceOutputs és globalParameters beállítások használt hello neveket hello kísérletekben hello neveket pontosan egyeznie kell. Az Azure ML végpont tooverify várt hello leképezés hello minta-kérések forgalma hello kötegelt végrehajtási súgó lapon tekintheti meg.

```JSON
{
    "name": "PredictivePipeline",
    "properties": {
        "description": "use AzureML model",
        "activities": [{
            "name": "MLActivity",
            "type": "AzureMLBatchExecution",
            "description": "prediction analysis on batch input",
            "inputs": [{
                "name": "inputDataset1"
            }, {
                "name": "inputDataset2"
            }],
            "outputs": [{
                "name": "outputDataset"
            }],
            "linkedServiceName": "MyAzureMLLinkedService",
            "typeProperties": {
                "webServiceInputs": {
                    "input1": "inputDataset1",
                    "input2": "inputDataset2"
                },
                "webServiceOutputs": {
                    "output1": "outputDataset"
                }
            },
            "policy": {
                "concurrency": 3,
                "executionPriorityOrder": "NewestFirst",
                "retry": 1,
                "timeout": "02:00:00"
            }
        }],
        "start": "2016-02-13T00:00:00Z",
        "end": "2016-02-14T00:00:00Z"
    }
}
```

#### <a name="web-service-does-not-require-an-input"></a>Webszolgáltatás nem szükséges bemeneti
Az Azure ML kötegelt végrehajtási webszolgáltatások lehet használt toorun munkafolyamatokat, például R vagy Python parancsfájlok, nem követelheti meg, amely a bemeneti adatok. Vagy hello kísérlet az olvasó modul, amely nem fedi fel minden GlobalParameters is megadhatók. Ebben az esetben hello AzureMLBatchExecution tevékenység úgy lesz beállítva, az alábbiak szerint:

```JSON
{
    "name": "scoring service",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "myBlob"
        }
    ],
    "typeProperties": {
        "webServiceOutputs": {
            "output1": "myBlob"
        }              
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-does-not-require-an-inputoutput"></a>Webszolgáltatás nem szükséges egy bemeneti/kimeneti
Azure ML kötegelt végrehajtási webszolgáltatás hello esetleg nincs konfigurálva webszolgáltatás kimenetet. Ebben a példában nincs webszolgáltatás bemeneti vagy kimeneti, sem bármely GlobalParameters vannak konfigurálva. Továbbra is maga hello tevékenység konfigurált kimenettel, de ez nem egy webServiceOutput van megadva.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "outputs": [
        {
            "name": "placeholderOutputDataset"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

#### <a name="web-service-uses-readers-and-writers-and-hello-activity-runs-only-when-other-activities-have-succeeded"></a>Webes szolgáltatás által használt olvasók és írók és hello tevékenység fut csak akkor, ha más tevékenységek sikeres volt
hello Azure ML web service írási és olvasási szerepkörökhöz modulok vagy bármely GlobalParameters nélkül konfigurált toorun lehet. Azonban érdemes lehet Szolgáltatáshívások tooembed egy folyamatot, amely a dataset függőségek tooinvoke hello szolgáltatást használ, csak akkor, ha egy fölérendelt feldolgozása befejeződött. Más műveleteket is el lehet indítani, hello kötegelt végrehajtáshoz a ezzel a megközelítéssel befejeződése után. Ebben az esetben is express hello függőségek tevékenység bemenetekhez és kimenetekhez, használja valamelyiket, a webszolgáltatás bemeneti vagy kimeneti megadása nélkül.

```JSON
{
    "name": "retraining",
    "type": "AzureMLBatchExecution",
    "inputs": [
        {
            "name": "upstreamData1"
        },
        {
            "name": "upstreamData2"
        }
    ],
    "outputs": [
        {
            "name": "downstreamData"
        }
    ],
    "typeProperties": {
     },
    "linkedServiceName": "mlEndpoint",
    "policy": {
        "concurrency": 1,
        "executionPriorityOrder": "NewestFirst",
        "retry": 1,
        "timeout": "02:00:00"
    }
},
```

Hello **takeaways** vannak:

* Ha a kísérlet végpont egy típus használ: egy blob-adathalmazra képviseli, és a hello tevékenység bemenetei és hello típus tulajdonság tartalmazza. Ellenkező esetben hello típus tulajdonság nincs megadva.
* Ha a kísérlet végpont használ webServiceOutput(s): blob adatkészletek képviseli, és tartalmazza a hello tevékenység kimeneteiből és hello webServiceOutputs tulajdonságában. hello tevékenység kimenete és webServiceOutputs minden kimenetet hello kísérletben hello név szerint vannak leképezve. Ellenkező esetben hello webServiceOutputs tulajdonság nincs megadva.
* Ha a kísérlet végpont globalParameter(s) azt mutatja, a számukra hello tevékenység globalParameters tulajdonságában kulcs, érték párként. Ellenkező esetben hello globalParameters tulajdonság nincs megadva. hello kulcsai kis-és nagybetűket. [Az Azure Data Factory funkciók](data-factory-functions-variables.md) hello értékek használható.
* További adathalmazokat szerepelni fog hello tevékenység bemenetekhez és kimenetekhez tulajdonságait, a hello tevékenység typeProperties rendelés nélkül. Ezek az adatkészletek szelet függőségek végrehajtását szabályozására, de hello AzureMLBatchExecution tevékenység ellenkező esetben figyelmen kívül hagyja.


## <a name="updating-models-using-update-resource-activity"></a>A modellek használata az Update-Erőforrástevékenység frissítése
Után átképezési befejezése, frissítse a webszolgáltatás (prediktív kísérletté webszolgáltatásként kitett) pontozási hello újonnan betanított modell hello segítségével hello **Azure ML Update Erőforrástevékenység**. Lásd: [modellek használata az Update-Erőforrástevékenység frissítése](data-factory-azure-ml-update-resource-activity.md) cikkben alább.

### <a name="reader-and-writer-modules"></a>Olvasási és írási modulok
Egy általános forgatókönyv webszolgáltatás-paramétereket az Azure SQL-olvasók és írók hello használata. hello olvasó modul az Azure Machine Learning Studio kívül adatszolgáltatásokból felügyeleti kísérlet használt tooload adatokat. hello író modul az adatok felügyeleti szolgáltatás Azure Machine Learning Studio kívül a kísérletekből toosave adatokat.  

Azure Blob vagy az Azure SQL olvasási/írási kapcsolatos részletekért lásd: [olvasó](https://msdn.microsoft.com/library/azure/dn905997.aspx) és [író](https://msdn.microsoft.com/library/azure/dn905984.aspx) témakörök MSDN könyvtárában. hello példa az előző szakaszban hello használt hello Azure Blob és Azure Blob. Ez a szakasz ismerteti az Azure SQL-olvasó és az Azure SQL-író segítségével.

## <a name="frequently-asked-questions"></a>Gyakori kérdések
**K:** a big Data típusú adatok folyamatok által létrehozott több fájl van. Használható összes hello fájljának hello AzureMLBatchExecution tevékenység toowork?

**V:** Igen. Lásd: hello **használatával egy olvasó modul tooread adatok Azure BLOB több fájlból** című szakaszban talál információt.

## <a name="azure-ml-batch-scoring-activity"></a>Azure ML kötegelt pontozási tevékenység
Hello használata **AzureMLBatchScoring** tevékenység toointegrate Azure Machine Learning segítségével, azt javasoljuk, hogy használjon hello legújabb **AzureMLBatchExecution** tevékenység.

hello AzureMLBatchExecution tevékenység hello Azure SDK és Azure PowerShell 2015. augusztus kiadása be.

Ha azt szeretné, hogy az hello AzureMLBatchScoring tevékenység toocontinue, továbbra is egész ebben a szakaszban.  

### <a name="azure-ml-batch-scoring-activity-using-azure-storage-for-inputoutput"></a>Bemeneti/kimeneti az Azure Storage használata az Azure ML kötegelt pontozási tevékenység

```JSON
{
  "name": "PredictivePipeline",
  "properties": {
    "description": "use AzureML model",
    "activities": [
      {
        "name": "MLActivity",
        "type": "AzureMLBatchScoring",
        "description": "prediction analysis on batch input",
        "inputs": [
          {
            "name": "ScoringInputBlob"
          }
        ],
        "outputs": [
          {
            "name": "ScoringResultBlob"
          }
        ],
        "linkedServiceName": "MyAzureMLLinkedService",
        "policy": {
          "concurrency": 3,
          "executionPriorityOrder": "NewestFirst",
          "retry": 1,
          "timeout": "02:00:00"
        }
      }
    ],
    "start": "2016-02-13T00:00:00Z",
    "end": "2016-02-14T00:00:00Z"
  }
}
```

### <a name="web-service-parameters"></a>Webszolgáltatás-paramétereket
Webszolgáltatás-paramétereket, toospecify értékeinek hozzáadása egy **typeProperties** szakasz toohello **AzureMLBatchScoringActivty** hello adatcsatorna JSON, ahogy az alábbi példa hello szakaszában:

```JSON
"typeProperties": {
    "webServiceParameters": {
        "Param 1": "Value 1",
        "Param 2": "Value 2"
    }
}
```
Is [Data Factory funkciók](data-factory-functions-variables.md) a sikeres hello értékeinek webszolgáltatás-paramétereket a hello a következő példában látható módon:

```JSON
"typeProperties": {
    "webServiceParameters": {
       "Database query": "$$Text.Format('SELECT * FROM myTable WHERE timeColumn = \\'{0:yyyy-MM-dd HH:mm:ss}\\'', Time.AddHours(WindowStart, 0))"
    }
}
```

> [!NOTE]
> Webszolgáltatás-paramétereket hello kis-és nagybetűket, ezért figyeljen oda arra, hogy hello hello tevékenységben megadott JSON megfelel hello hello webszolgáltatás által elérhetővé tett néhányat a meglévők közül.
>
>

## <a name="see-also"></a>Lásd még:
* [Az Azure blogbejegyzést: Ismerkedés az Azure Data Factory és az Azure gépi tanulás](https://azure.microsoft.com/blog/getting-started-with-azure-data-factory-and-azure-machine-learning-4/)

[adf-build-1st-pipeline]: data-factory-build-your-first-pipeline.md

[azure-machine-learning]: http://azure.microsoft.com/services/machine-learning/
