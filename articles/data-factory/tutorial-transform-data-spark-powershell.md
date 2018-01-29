---
title: "Adatok átalakítása a Spark segítségével az Azure Data Factoryban | Microsoft Docs"
description: "Ez az oktatóanyag lépésenkénti utasításokat biztosít az adatok átalakításához egy Spark-tevékenység az Azure Data Factoryban való használatával."
services: data-factory
documentationcenter: 
author: shengcmsft
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/22/2018
ms.author: shengc
ms.openlocfilehash: af5e19b212fdebe5220e49eeaa6ec9fc56b1da1c
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/23/2018
---
# <a name="transform-data-in-the-cloud-by-using-spark-activity-in-azure-data-factory"></a>Adatátalakítás a felhőben egy Spark-tevékenység az Azure Data Factoryban való használatával
Ebben az oktatóanyagban az Azure PowerShell segítségével hozhat létre egy Data Factory-folyamatot, amely egy Spark-tevékenységgel és egy igény szerinti HDInsight társított szolgáltatással alakítja át az adatokat. Az oktatóanyagban az alábbi lépéseket fogja végrehajtani:

> [!div class="checklist"]
> * Adat-előállító létrehozása 
> * Társított szolgáltatások készítése és üzembe helyezése
> * Folyamat létrehozása és üzembe helyezése. 
> * Folyamat futásának indítása
> * A folyamat futásának monitorozása.

> [!NOTE]
> Ez a cikk a Data Factory 2. verziójára vonatkozik, amely jelenleg előzetes verzióban érhető el. Ha a Data Factory szolgáltatás általánosan elérhető 1. verzióját használja, lásd [a Data Factory 1. verziójának dokumentációját](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Ha nem rendelkezik Azure-előfizetéssel, első lépésként mindössze néhány perc alatt létrehozhat egy [ingyenes](https://azure.microsoft.com/free/) fiókot.

## <a name="prerequisites"></a>Előfeltételek
* **Azure Storage-fiók** Létrehoz egy Python-szkriptet és egy bemeneti fájlt, és feltölti őket az Azure Storage-ba. A Spark-program kimenetét ebben a tárfiókban tárolja a rendszer. Az igény szerinti Spark-fürt ugyanezt a tárfiókot használja elsődleges tárterületként.  
* **Azure PowerShell**. Kövesse [az Azure PowerShell telepítését és konfigurálását](/powershell/azure/install-azurerm-ps) ismertető cikkben szereplő utasításokat.


### <a name="upload-python-script-to-your-blob-storage-account"></a>Python-szkript feltöltése a Blob Storage-fiókba
1. Hozzon létre egy **WordCount_Spark.py** nevű Python-fájlt az alábbi tartalommal: 

    ```python
    import sys
    from operator import add
    
    from pyspark.sql import SparkSession
    
    def main():
        spark = SparkSession\
            .builder\
            .appName("PythonWordCount")\
            .getOrCreate()
            
        lines = spark.read.text("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/inputfiles/minecraftstory.txt").rdd.map(lambda r: r[0])
        counts = lines.flatMap(lambda x: x.split(' ')) \
            .map(lambda x: (x, 1)) \
            .reduceByKey(add)
        counts.saveAsTextFile("wasbs://adftutorial@<storageaccountname>.blob.core.windows.net/spark/outputfiles/wordcount")
        
        spark.stop()
    
    if __name__ == "__main__":
        main()
    ```
2. Cserélje a **&lt;storageAccountName&gt;** kifejezést Azure Storage-fiókja nevére. Ezután mentse a fájlt. 
3. Az Azure Blob Storage-ban hozzon létre egy **adftutorial** nevű tárolót, ha még nem létezik. 
4. Hozzon létre egy **spark** mappát.
5. Hozzon létre egy **szkript** almappát a **spark** mappában. 
6. Töltse fel a **WordCount_Spark.py** fájlt a **szkript** almappába. 


### <a name="upload-the-input-file"></a>A bemeneti fájl feltöltése
1. Hozzon létre egy **minecraftstory.txt** nevű fájlt némi szöveges tartalommal. A Spark-program megszámolja a szavak számát ebben a szövegben. 
2. Hozzon létre egy `inputfiles` nevű almappát a `spark` mappában. 
3. Töltse fel a `minecraftstory.txt` fájlt az `inputfiles` almappába. 

## <a name="author-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a szakaszban két társított szolgáltatást hoz létre: 
    
- Egy Azure Storage-beli társított szolgáltatást, amely egy Azure Storage-fiókot társít az adat-előállítóhoz. Ezt a tárterületet csak az igény szerinti HDInsight-fürt használja. Ez tartalmazza a végrehajtani kívánt Spark-szkriptet is. 
- Egy igény szerinti HDInsight társított szolgáltatást. Az Azure Data Factory automatikusan létrehoz egy HDInsight-fürtöt, futtatja a Spark-programot, majd törli a HDInsight-fürtöt, ha az egy előre megadott időtartamon keresztül inaktív. 

### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
Hozzon létre egy JSON-fájlt az előnyben részesített szerkesztővel, másolja a fájlba egy Azure Storage társított szolgáltatás alábbi JSON-definícióját, majd mentse a fájlt **MyStorageLinkedService.json** néven.  

```json
{
    "name": "MyStorageLinkedService",
    "properties": {
      "type": "AzureStorage",
      "typeProperties": {
        "connectionString": {
          "value": "DefaultEndpointsProtocol=https;AccountName=<storageAccountName>;AccountKey=<storageAccountKey>",
          "type": "SecureString"
        }
      }
    }
}
```
Cserélje le a &lt;storageAccountName&gt; és a &lt;storageAccountKey&gt; értékeket a saját Azure Storage-fiókja nevére és kulcsára. 


### <a name="on-demand-hdinsight-linked-service"></a>Igény szerinti HDInsight társított szolgáltatás
Hozzon létre egy JSON-fájlt az előnyben részesített szerkesztővel, másolja a fájlba egy Azure HDInsight társított szolgáltatás alábbi JSON-definícióját, majd mentse a fájlt **MyOnDemandSparkLinkedService.json** néven.  

```json
{
    "name": "MyOnDemandSparkLinkedService",
    "properties": {
      "type": "HDInsightOnDemand",
      "typeProperties": {
        "clusterSize": 2,
        "clusterType": "spark",
        "timeToLive": "00:15:00",
        "hostSubscriptionId": "<subscriptionID> ",
        "servicePrincipalId": "<servicePrincipalID>",
        "servicePrincipalKey": {
          "value": "<servicePrincipalKey>",
          "type": "SecureString"
        },
        "tenant": "<tenant ID>",
        "clusterResourceGroup": "<resourceGroupofHDICluster>",
        "version": "3.6",
        "osType": "Linux",
        "clusterNamePrefix":"ADFSparkSample",
        "linkedServiceName": {
          "referenceName": "MyStorageLinkedService",
          "type": "LinkedServiceReference"
        }
      }
    }
}
```
Frissítse a következő tulajdonságok értékeit a társított szolgáltatás definíciójában: 

- **hostSubscriptionId**. Cserélje le a &lt;SubscriptionId&gt; kifejezést az Azure-előfizetés azonosítójára. Létrejön az igény szerinti HDInsight-fürt ebben az előfizetésben. 
- **tenant**. A &lt;tenantID&gt; helyére írja a saját Azure bérlőjének az azonosítóját. 
- **servicePrincipalId**, **servicePrincipalKey**. A &lt;servicePrincipalID&gt; és a &lt;servicePrincipalKey&gt; helyére írja az Azure Active Directoryban használatos szolgáltatásneve azonosítóját és kulcsát. A szolgáltatásnévnek az előfizetés vagy a létrejövő fürtnek helyet adó erőforráscsoport Közreműködő szerepkörének tagjának kell lennie. Részletek: [Azure Active Directory-alkalmazás és -szolgáltatásnév létrehozása](../azure-resource-manager/resource-group-create-service-principal-portal.md). 
- **clusterResourceGroup**. A &lt;resourceGroupOfHDICluster&gt; helyére írja annak az erőforráscsoportnak a nevét, amelyben a HDInsight-fürtöt létre kell hozni. 

> [!NOTE]
> Az Azure HDInsightban korlátozott azon magok száma, amelyek az egyes támogatott Azure-régiókban felhasználhatók. Igény szerinti HDInsight társított szolgáltatás esetében a HDInsight-fürt ugyanazon a helyen jön létre, mint amit az Azure Storage elsődleges tárterületként használ. Ellenőrizze, hogy a magkvótája elegendő-e a fürt sikeres létrehozásához. További információk: [Fürtök beállítása a HDInsightban Hadoop, Spark, Kafka stb. használatával](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md). 


## <a name="author-a-pipeline"></a>Folyamat létrehozása 
Ebben a lépésben létrehoz egy új folyamatot egy Spark-tevékenységgel. A tevékenység a **szószámlálás** példát használja. Ha még nem tette meg, töltse le a tartalmakat innen.

Hozzon létre egy JSON-fájlt az előnyben részesített szerkesztővel, másolja a fájlba egy folyamatdefiníció alábbi JSON-definícióját, majd mentse a fájlt **MySparkOnDemandPipeline.json** néven. 

```json
{
  "name": "MySparkOnDemandPipeline",
  "properties": {
    "activities": [
      {
        "name": "MySparkActivity",
        "type": "HDInsightSpark",
        "linkedServiceName": {
            "referenceName": "MyOnDemandSparkLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
          "rootPath": "adftutorial/spark",
          "entryFilePath": "script/WordCount_Spark.py",
          "getDebugInfo": "Failure",
          "sparkJobLinkedService": {
            "referenceName": "MyStorageLinkedService",
            "type": "LinkedServiceReference"
          }
        }
      }
    ]
  }
}
```

Vegye figyelembe a következő szempontokat: 

- A rootPath az adftutorial tároló spark mappájára mutat. 
- Az entryFilePath a spark mappa szkript almappájában található WordCount_Spark.py fájlra mutat. 


## <a name="create-a-data-factory"></a>Data factory létrehozása 
Már létrehozta a társított szolgáltatást és a folyamat definícióját JSON-fájlokban. Most hozzunk létre egy adat-előállítót, és helyezzük üzembe a társított szolgáltatás és a folyamat JSON-fájljait PowerShell-parancsmagok segítségével. Futtassa egyesével az alábbi PowerShell-parancsokat: 

1. Adja meg egyesével a változókat.

    **Erőforráscsoport neve**
    ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup" 
    ```

    **Adat-előállító neve. A névnek globálisan egyedinek kell lennie** 
    ```powershell
    $dataFactoryName = "MyDataFactory09102017"
    ```
    
    **Folyamat neve**
    ```powershell
    $pipelineName = "MySparkOnDemandPipeline" # Name of the pipeline
    ```
2. Indítsa el a **PowerShellt**. Az Azure PowerShellt hagyja megnyitva a rövid útmutató végéig. Ha bezárja és újra megnyitja a programot, akkor újra le kell futtatnia a parancsokat. A Data Factory 2-es verziója jelenleg csak az USA keleti régiójában, az USA 2. keleti régiójában és a nyugat-európai régióban teszi lehetővé adat-előállítók létrehozását. Az adat-előállítók által használt adattárak (Azure Storage, Azure SQL Database stb.) és számítási erőforrások (HDInsight stb.) más régiókban is lehetnek.

    Futtassa a következő parancsot, és adja meg az Azure Portalra való bejelentkezéshez használt felhasználónevet és jelszót.
        
    ```powershell
    Login-AzureRmAccount
    ```        
    Futtassa a következő parancsot a fiókhoz tartozó előfizetések megtekintéséhez.

    ```powershell
    Get-AzureRmSubscription
    ```
    Futtassa a következő parancsot a használni kívánt előfizetés kiválasztásához. Cserélje le a **SubscriptionId** kifejezést az Azure-előfizetés azonosítójára:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"    
    ```  
3. Hozza létre az ADFTutorialResourceGroup erőforráscsoportot. 

    ```powershell
    New-AzureRmResourceGroup -Name $resourceGroupName -Location "East Us" 
    ```
4. Hozza létre az adat-előállítót. 

    ```powershell
     $df = Set-AzureRmDataFactoryV2 -Location EastUS -Name $dataFactoryName -ResourceGroupName $resourceGroupName
    ```

    A kimenet megtekintéséhez futtassa a következő parancsot: 

    ```powershell
    $df
    ```
5. Váltson arra a mappára, ahol létrehozta a JSON-fájlokat, és futtassa az alábbi parancsot egy Azure Storage társított szolgáltatás üzembe helyezéséhez: 
       
    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyStorageLinkedService" -File "MyStorageLinkedService.json"
    ```
6. Futtassa az alábbi parancsot egy igény szerinti Spark társított szolgáltatás üzembe helyezéséhez: 
       
    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "MyOnDemandSparkLinkedService" -File "MyOnDemandSparkLinkedService.json"
    ```
7. Futtassa az alábbi parancsot egy folyamat üzembe helyezéséhez: 
       
    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name $pipelineName -File "MySparkOnDemandPipeline.json"
    ```
    
## <a name="start-and-monitor-a-pipeline-run"></a>Folyamat futásának indítása és monitorozása  

1. Folyamat futásának indítása Így megőrizheti a folyamat futásának azonosítóját későbbi monitorozás céljából.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName $pipelineName  
    ```
2. A folyamatfuttatás állapotának a befejezésig való folyamatos ellenőrzéséig futtassa az alábbi szkriptet.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    
        if(!$result) {
            Write-Host "Waiting for pipeline to start..." -foregroundcolor "Yellow"
        }
        elseif (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
        }
        else {
            Write-Host "Pipeline '"$pipelineName"' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
        ($result | Format-List | Out-String)
        Start-Sleep -Seconds 15
    }

    Write-Host "Activity `Output` section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"

    Write-Host "Activity `Error` section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n" 
    ```  
3. Itt látható a példa futtatás kimenete: 

    ```
    Pipeline run status: In Progress
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : 
    ActivityName      : MySparkActivity
    PipelineRunId     : 94e71d08-a6fa-4191-b7d1-cf8c71cb4794
    PipelineName      : MySparkOnDemandPipeline
    Input             : {rootPath, entryFilePath, getDebugInfo, sparkJobLinkedService}
    Output            : 
    LinkedServiceName : 
    ActivityRunStart  : 9/20/2017 6:33:47 AM
    ActivityRunEnd    : 
    DurationInMs      : 
    Status            : InProgress
    Error             :
    …
    
    Pipeline ' MySparkOnDemandPipeline' run finished. Result:
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : MyDataFactory09102017
    ActivityName      : MySparkActivity
    PipelineRunId     : 94e71d08-a6fa-4191-b7d1-cf8c71cb4794
    PipelineName      : MySparkOnDemandPipeline
    Input             : {rootPath, entryFilePath, getDebugInfo, sparkJobLinkedService}
    Output            : {clusterInUse, jobId, ExecutionProgress, effectiveIntegrationRuntime}
    LinkedServiceName : 
    ActivityRunStart  : 9/20/2017 6:33:47 AM
    ActivityRunEnd    : 9/20/2017 6:46:30 AM
    DurationInMs      : 763466
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity Output section:
    "clusterInUse": "https://ADFSparkSamplexxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.azurehdinsight.net/"
    "jobId": "0"
    "ExecutionProgress": "Succeeded"
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)"
    Activity Error section:
    "errorCode": ""
    "message": ""
    "failureType": ""
    "target": "MySparkActivity"
    ```
4. Ellenőrizze, hogy az `outputfiles` nevű mappa létrejött-e az adftutorial tároló `spark` mappájában a Spark program kimenetével. 


## <a name="next-steps"></a>További lépések
A példában szereplő folyamat adatokat másol az egyik helyről egy másikra egy Azure Blob Storage-ban. Megismerte, hogyan végezheti el az alábbi műveleteket: 

> [!div class="checklist"]
> * Adat-előállító létrehozása 
> * Társított szolgáltatások készítése és üzembe helyezése
> * Folyamat létrehozása és üzembe helyezése. 
> * Folyamat futásának indítása
> * A folyamat futásának monitorozása.

A következő oktatóanyagra lépve megtudhatja, hogyan alakíthat át adatokat egy Hive szkriptnek a virtuális hálózatban lévő Azure HDInsight-fürtön való futtatásával. 

> [!div class="nextstepaction"]
> [Oktatóanyag: adatok átalakítása a Hive használatával egy Azure virtuális hálózatban](tutorial-transform-data-hive-virtual-network.md).





