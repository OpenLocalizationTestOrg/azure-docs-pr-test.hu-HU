---
title: "a Stream Analytics aaaUse Azure Machine Learning-végpont |} Microsoft Docs"
description: "A Stream Analytics gép nyelvi felhasználó által definiált függvények"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 406b258f-b8c2-4e55-953c-b7f84e8e5354
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 013b841ee85b1e0b6d8139a9ba0dde88fc3f8ad0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="machine-learning-integration-in-stream-analytics"></a>A Stream Analytics tanulási integrációs számítógép
A Stream Analytics támogatja a felhasználó által definiált függvények, amelyek tooAzure Machine Learning-végpont hívásához. Ez a szolgáltatás REST API-támogatása részleteit a hello [Stream Analytics REST API-könyvtár](https://msdn.microsoft.com/library/azure/dn835031.aspx). Ez a cikk ezt a képességet a Stream Analytics sikeres végrehajtásához szükség kiegészítő információkat tartalmazza. Oktatóanyag is közzé lett, és elérhető [Itt](stream-analytics-machine-learning-integration-tutorial.md).

## <a name="overview-azure-machine-learning-terminology"></a>Áttekintés: Azure Machine Learning-terminológia
A Microsoft Azure Machine Learning használható együttműködést támogató, egérrel húzási és elengedési eszközt biztosít toobuild, tesztelését és rendszerbe adataihoz prediktív elemzési megoldások. Ez az eszköz neve hello *Azure Machine Learning Studio*. hello studio használt toointeract hello a Machine Learning erőforrások és egyszerűen hozhatók létre, tesztelheti, és a Tervező többször. Ezeket az erőforrásokat és a definíciójukat alatt van.

* **Munkaterület**: hello *munkaterület* egy olyan tároló, amely tárolja az összes többi Machine Learning erőforrása együtt a felügyeleti és ellenőrzési tárolója.
* **Kísérlet**: *kísérletek* adatok kutatók tooutilize adatkészletek hozza létre, és a gépi tanulási modell betanításához.
* **Végpont**: *végpontok* hello Azure Machine Learning használt objektum tootake szolgáltatások bemeneti adatként, alkalmazza a megadott gépi tanulási modell, és a pontozott kimenet visszaadása.
* **Webszolgáltatás pontozási**: A *webservice pontozási* olyan végpontok gyűjteménye, fent említett.

Minden egyes végpont API-k vannak a kötegelt végrehajtási és szinkron végrehajtása. A Stream Analytics szinkron végrehajtási használja. hello adott szolgáltatás nevű egy [kérelem-válasz szolgáltatás](../machine-learning/machine-learning-consume-web-services.md) AzureML Studio.

## <a name="machine-learning-resources-needed-for-stream-analytics-jobs"></a>Gépi tanulási, amelyekre szükség van a Stream Analytics-feladatok
A Stream Analytics hello alkalmazásában feladat-feldolgozás, a kérelem/válasz végpont egy [apikey](../machine-learning/machine-learning-connect-to-azure-machine-learning-web-service.md), és a swagger-definíciójában minden szükséges, a sikeres végrehajtáshoz. A Stream Analytics egy további végpontot, amely swagger-végpont URL-címe hello hoz létre, hello felületet és visszaadása egy alapértelmezett UDF definition toohello felhasználó rendelkezik.

## <a name="configure-a-stream-analytics-and-machine-learning-udf-via-rest-api"></a>Konfiguráljon egy Stream Analytics és a gépi tanulási UDF REST API-n keresztül
A feladat toocall Azure Machine nyelvi funkciókat konfigurálhatja REST API-k használatával. hello lépései a következők:

1. Stream Analytics-feladat létrehozása
2. Adja meg a bemeneti
3. Adja meg egy kimeneti
4. Hozzon létre egy felhasználói függvény (UDF)
5. Írás a Stream Analytics átalakítás, hogy a hívások hello UDF-ben
6. Hello feladat indítása

## <a name="creating-a-udf-with-basic-properties"></a>Egy UDF alaptulajdonságait létrehozása
Tegyük fel, a következő példakód hello létrehoz egy skaláris UDF nevű *newudf* , amely tooan Azure Machine Learning-végpont van kötve. Vegye figyelembe, hogy hello *végpont* (URI-szolgáltatás) található a kiválasztott szolgáltatás- és hello hello hello API súgólapján *apiKey* hello szolgáltatások fő lapján található.

````
    PUT : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>  
````

Példa kérelemtörzset:  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77fb4b46bf2a30c63c078dca/services/b7be5e40fd194258796fb402c1958eaf/execute ",
                        "apiKey": "replacekeyhere"
                    }
                }
            }
        }
    }
````

## <a name="call-retrievedefaultdefinition-endpoint-for-default-udf"></a>Alapértelmezett UDF RetrieveDefaultDefinition végpont meghívása
Egyszer hello vázat UDF jön létre a teljes definíciója hello hello UDF van szükség. hello RetreiveDefaultDefinition végpont segít skaláris függvényt, amely kötött tooan Azure Machine Learning-végpont hello alapértelmezett definíciója. hello hasznos az alábbi szükséges tooget hello alapértelmezett UDF definition skaláris függvény, amely kötött tooan Azure Machine Learning-végpont esetében. Mivel azt már megadott PUT kérelem során, azt az hello tényleges végpontja nem ad meg. A Stream Analytics meghívja a hello kérelemben megadott, ha explicit módon megadott hello végpont. Ellenkező esetben egy eredetileg hivatkozott hello használ. Itt hello egy karakterlánc-paramétert (mondat), és értéket ad vissza, egyetlen kimeneti karakterlánc típusú, ami azt jelzi, hogy a mondatok hello "véleményeket" címke UDF vesz igénybe.

````
POST : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>/RetrieveDefaultDefinition?api-version=<apiVersion>
````

Példa kérelemtörzset:  

````
    {
        "bindingType": "Microsoft.MachineLearning/WebService",
        "bindingRetrievalProperties": {
            "executeEndpoint": null,
            "udfType": "Scalar"
        }
    }
````

A kimeneti minta e keresse meg valami szeretné alatt.  

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="patch-udf-with-hello-response"></a>Javítás UDF hello válasz
Most hello UDF kell javítani hello előző válaszban, az alább látható módon.

````
PATCH : /subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.StreamAnalytics/streamingjobs/<streamingjobName>/functions/<udfName>?api-version=<apiVersion>
````

A kérelem törzsében (RetrieveDefaultDefinition kimenete):

````
    {
        "name": "newudf",
        "properties": {
            "type": "Scalar",
            "properties": {
                "inputs": [{
                    "dataType": "nvarchar(max)",
                    "isConfigurationParameter": null
                }],
                "output": {
                    "dataType": "nvarchar(max)"
                },
                "binding": {
                    "type": "Microsoft.MachineLearning/WebService",
                    "properties": {
                        "endpoint": "https://ussouthcentral.services.azureml.net/workspaces/f80d5d7a77ga4a4bbf2a30c63c078dca/services/b7be5e40fd194258896fb602c1858eaf/execute",
                        "apiKey": null,
                        "inputs": {
                            "name": "input1",
                            "columnNames": [{
                                "name": "tweet",
                                "dataType": "string",
                                "mapTo": 0
                            }]
                        },
                        "outputs": [{
                            "name": "Sentiment",
                            "dataType": "string"
                        }],
                        "batchSize": 10
                    }
                }
            }
        }
    }
````

## <a name="implement-stream-analytics-transformation-toocall-hello-udf"></a>A Stream Analytics átalakítása toocall hello UDF megvalósítása
Most lekérdezéséhez hello (Itt nevű scoreTweet) UDF minden bemeneti esemény és az adott esemény tooan kimeneti választ.  

````
    {
        "name": "transformation",
        "properties": {
            "streamingUnits": null,
            "query": "select *,scoreTweet(Tweet) TweetSentiment into blobOutput from blobInput"
        }
    }
````


## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)
