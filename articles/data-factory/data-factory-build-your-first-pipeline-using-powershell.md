---
title: "aaaBuild az első adat-előállítóban (PowerShell) |} Microsoft Docs"
description: "Az oktatóanyag során létrehoz egy minta Azure Data Factory-folyamatot az Azure PowerShell használatával."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 22ec1236-ea86-4eb7-b903-0e79a58b90c7
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 626260798b56d590577b3c4b24f7cf52873c9f80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-azure-powershell"></a>Oktatóanyag: Az első Azure data factory létrehozása az Azure PowerShell használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>

Ez a cikk használhatja az Azure PowerShell toocreate az első az Azure data factory. toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.

Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**. Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt. hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után. 

> [!NOTE]
> hello adatok csővezeték ebben az oktatóanyagban alakítja át a bemeneti adatok tooproduce kimeneti adatokat. Azt nem másolja az adatokat egy forrás adatokat tároló tooa cél adattárból. Hogyan oktatóanyagért toocopy adatok Azure Data Factory használatával, lásd: [oktatóanyag: adatok másolása a Blob Storage tooSQL adatbázis](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
> 
> Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).

## <a name="prerequisites"></a>Előfeltételek
* Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.
* Kövesse az utasításokat [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview) cikk tooinstall legújabb verziójának Azure PowerShell a számítógépen.
* (választható) Ez a cikk nem foglalkozik minden hello Data Factory-parancsmag. A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentációért tekintse meg a [Data Factory Cmdlet Reference](/powershell/module/azurerm.datafactories) (Data Factory-parancsmagok referenciája) című cikket.

## <a name="create-data-factory"></a>Data factory létrehozása
Ezt a lépést használhatja az Azure PowerShell toocreate egy Azure Data Factory nevű **FirstDataFactoryPSH**. A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Adja meg például a másolási tevékenység toocopy forrás tooa cél adattárat és adatait egy HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat. Kezdjük, az ebben a lépésben hello adat-előállító létrehozása.

1. Indítsa el az Azure PowerShell, és futtassa a következő parancs hello. Azure PowerShell nyitva hagyja az oktatóanyag hello végéig. Zárja be és nyissa meg újra, ha szüksége toorun ezek a parancsok újra.
   * Futtassa a következő parancs hello, és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.
    ```PowerShell
    Login-AzureRmAccount
    ```    
   * Futtassa a következő parancs tooview hello minden hello előfizetés ehhez a fiókhoz.
    ```PowerShell
    Get-AzureRmSubscription 
    ```
   * Futtassa a következő parancs tooselect hello előfizetést, amelyet a toowork hello. Ez az előfizetés ugyanaz, mint egy Azure-portálon hello használt hello kell hello.
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```     
2. Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello a következő parancs futtatásával:
    
    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```
    Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát. Ha egy másik erőforráscsoportban található használatához szüksége van-e toouse helyett, ebben az oktatóanyagban ADFTutorialResourceGroup azt.
3. Futtassa a hello **New-AzureRmDataFactory** parancsmag által létrehozott egy adat-előállító nevű **FirstDataFactoryPSH**.

    ```PowerShell
    New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH –Location "West US"
    ```
Vegye figyelembe a következő pontok hello:

* Azure Data Factory hello hello nevének globálisan egyedi kell lennie. Ha hello hibaüzenet **nem érhető el adat-előállító "FirstDataFactoryPSH"**, módosítsa hello nevét (például yournameFirstDataFactoryPSH). Használja ezt az ADFTutorialFactoryPSH helyett az oktatóanyag lépéseinek végrehajtása során. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
* toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda
* hello adat-előállító nevét hello előfordulhat, hogy jövőbeli hello a DNS-névként regisztrált, és ezért a nyilvánosan láthatóvá válnak.
* Ha hello hibaüzenet: "**ehhez az előfizetéshez nincs regisztrált toouse névtér Microsoft.DataFactory**", hello alábbi, és próbálja meg újra közzétenni:

  * Az Azure PowerShell futtassa a következő parancs tooregister hello adat-előállító szolgáltató hello:

    ```PowerShell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```
      A következő parancs tooconfirm hello adott hello szolgáltató regisztrálva van a Data Factory futtathatja:

    ```PowerShell
    Get-AzureRmResourceProvider
    ```
  * Bejelentkezés használatával hello Azure-előfizetéssel való hello [Azure-portálon](https://portal.azure.com) , és keresse meg a tooa adat-előállító panel (vagy) hozzon létre egy adat-előállító hello Azure-portálon. Ez a művelet automatikusan regisztrálja az Ön hello szolgáltató.

Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először. Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolására, adja meg a bemeneti és kimeneti adatkészletek toorepresent bemeneti/kimeneti adatai csatolt adatok áruházakból és majd hozzon létre egy tevékenység által használt ezek az adatkészletek hello folyamat.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban. hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók. HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl. Azonosíthatja az adatokat tároló/számítási szolgáltatások a forgatókönyvben használt, és ezen szolgáltatások toohello adat-előállító hivatkozás összekapcsolt szolgáltatások létrehozásával.

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra. Hello használata azonos Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.

1. A következő hello hello C:\ADFGetStarted mappában StorageLinkedService.json nevű JSON-fájl tartalmának létrehozása. Hozzon létre hello mappát ADFGetStarted, ha még nem létezik.

    ```json
    {
        "name": "StorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "description": "",
            "typeProperties": {
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        }
    }
    ```
    Cserélje le **fióknév** hello nevű Azure-tárfiókot és **fiókkulcs** kulcsával hello hozzáférés hello Azure storage-fiók. toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
2. Azure PowerShell, a kapcsoló toohello ADFGetStarted mappa.
3. Használhatja a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott összekapcsolt szolgáltatás. Ez a parancsmag és egyéb adat-előállító parancsmagok használata ebben az oktatóanyagban meg toopass paraméterértékek szükségesek hello *ResourceGroupName* és *DataFactoryName* paraméterek. Másik megoldásként használhatja **Get-AzureRmDataFactory** tooget egy **DataFactory** objektumot, és adja át a hello objektum beírása nélkül *ResourceGroupName* és  *DataFactoryName* minden alkalommal, amikor futtatja a parancsmagot. Futtatási hello következő parancsot a tooassign hello kimenete hello **Get-AzureRmDataFactory** parancsmag tooa **$df** változó.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
4. Most, futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott hello kapcsolódó **StorageLinkedService** szolgáltatás.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\StorageLinkedService.json
    ```
    Hello volt futtatásakor **Get-AzureRmDataFactory** parancsmag és a hozzárendelt hello kimeneti toohello **$df** változó, akkor egy hello toospecify értékeinek *ResourceGroupName*és *DataFactoryName* paramétereket az alábbiak szerint.

    ```PowerShell
    New-AzureRmDataFactoryLinkedService -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName FirstDataFactoryPSH -File .\StorageLinkedService.json
    ```
    Ha bezárja az Azure PowerShell hello oktatóanyag hello középső, rendelkezik-e toorun hello **Get-AzureRmDataFactory** parancsmag Azure PowerShell toocomplete hello oktatóanyag következő indításakor.

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra. az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz. Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat. További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).

1. Hozzon létre egy JSON fájlt **HDInsightOnDemandLinkedService**a hello .JSON kiterjesztésű **C:\ADFGetStarted** hello tartalom a következő mappában.

    ```json
    {
        "name": "HDInsightOnDemandLinkedService",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "linkedServiceName": "StorageLinkedService"
            }
        }
    }
    ```
    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | ClusterSize |HDInsight-fürt hello hello méretét adja meg. |
   | TimeToLive |Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt. |
   | linkedServiceName |Adja meg, amely a HDInsight által létrehozott használt toostore hello naplók hello storage-fiók |

    Vegye figyelembe a következő pontok hello:

   * hello adat-előállító létrehoz egy **Linux-alapú** a hello JSON meg HDInsight-fürthöz. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
   * Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat. További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).
   * hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Igény szerinti HDInsight társított szolgáltatás esetén a rendszer a szeletek feldolgozásakor mindig létrehoz egy HDInsight-fürtöt, kivéve, ha van meglévő élő fürt (**timeToLive**). hello feldolgozása hello fürt automatikusan törlődnek.

       Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

     További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
2. Futtassa a hello **New-AzureRmDataFactoryLinkedService** parancsmag által létrehozott hello társított HDInsightOnDemandLinkedService nevű szolgáltatás.
    
    ```PowerShell
    New-AzureRmDataFactoryLinkedService $df -File .\HDInsightOnDemandLinkedService.json
    ```

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra. Ezek az adatkészletek tekintse meg a toohello **StorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott. hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
1. Hozzon létre egy JSON fájlt **InputTable.json** a hello **C:\ADFGetStarted** hello tartalom a következő mappában:

    ```json
    {
        "name": "AzureBlobInput",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "StorageLinkedService",
            "typeProperties": {
                "fileName": "input.log",
                "folderPath": "adfgetstarted/inputdata",
                "format": {
                    "type": "TextFormat",
                    "columnDelimiter": ","
                }
            },
            "availability": {
                "frequency": "Month",
                "interval": 1
            },
            "external": true,
            "policy": {}
        }
    }
    ```
    hello JSON meghatározása nevű adatkészlete **AzureBlobInput**, amely jelenti, hogy egy tevékenység hello folyamat a bemeneti adatok. Emellett meghatározza, hogy hello bemeneti adatok nevű hello blob tárolóban található **adfgetstarted** és nevű hello mappát **inputdata**.

    hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

   | Tulajdonság | Leírás |
   |:--- |:--- |
   | type |mivel az adatok találhatók az Azure blob storage hello type tulajdonság beállítása tooAzureBlob. |
   | linkedServiceName |a korábban létrehozott StorageLinkedService toohello hivatkozik. |
   | fileName |Ez a tulajdonság nem kötelező. Ha ez a tulajdonság nincs megadva, az összes hello fájlok hello folderPath leltárhoz. Ebben az esetben csak hello input.log dolgoz fel. |
   | type |hello naplófájlok szöveges formátumú, így szöveges használjuk. |
   | columnDelimiter |oszlopok hello naplófájlokban határolja hello. vesszővel (,) karakter. |
   | frequency/interval |gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi. |
   | external |Ez a tulajdonság beállítása tootrue hello bemeneti adatok nem generálja hello Data Factory szolgáltatásnak. |
2. Futtassa a következő parancs az Azure PowerShell toocreate hello adat-előállító dataset hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\InputTable.json
    ```

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Hello kimeneti adatkészlet toorepresent hello kimeneti tárolt adatok hello Azure Blob-tároló létrehozása

1. Hozzon létre egy JSON fájlt **OutputTable.json** a hello **C:\ADFGetStarted** hello tartalom a következő mappában:

    ```json
    {
      "name": "AzureBlobOutput",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
          "folderPath": "adfgetstarted/partitioneddata",
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
    hello JSON meghatározása nevű adatkészlete **AzureBlobOutput**, amely jelenti, hogy egy tevékenység hello folyamat kimeneti adatokat. Emellett meghatározza, hogy hello eredmények nevű hello blob tárolóban kell tárolni **adfgetstarted** és nevű hello mappát **partitioneddata**. Hello **rendelkezésre állási** szakaszban határozza meg, hogy hello kimeneti adatkészlet havonta jön létre.
2. Futtassa a következő parancs az Azure PowerShell toocreate hello adat-előállító dataset hello:

    ```PowerShell
    New-AzureRmDataFactoryDataset $df -File .\OutputTable.json
    ```

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát. Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly. hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie. Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet. a következő JSON hello használt hello tulajdonságok hello végén ebben a szakaszban lévő magyarázatát olvashatja.

1. A következő hello hello C:\ADFGetStarted mappában MyFirstPipelinePSH.json nevű JSON-fájl tartalmának létrehozása:

   > [!IMPORTANT]
   > Cserélje le **storageaccountname** hello nevű hello JSON a storage-fiókot.
   >
   >

    ```json
    {
        "name": "MyFirstPipeline",
        "properties": {
            "description": "My first Azure Data Factory pipeline",
            "activities": [
                {
                    "type": "HDInsightHive",
                    "typeProperties": {
                        "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                        "scriptLinkedService": "StorageLinkedService",
                        "defines": {
                            "inputtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/inputdata",
                            "partitionedtable": "wasb://adfgetstarted@<storageaccountname>.blob.core.windows.net/partitioneddata"
                        }
                    },
                    "inputs": [
                        {
                            "name": "AzureBlobInput"
                        }
                    ],
                    "outputs": [
                        {
                            "name": "AzureBlobOutput"
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
                    "linkedServiceName": "HDInsightOnDemandLinkedService"
                }
            ],
            "start": "2017-07-01T00:00:00Z",
            "end": "2017-07-02T00:00:00Z",
            "isPaused": false
        }
    }
    ```
    Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatokat a HDInsight-fürtök létrehozásához.

    hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **StorageLinkedService**), majd a **parancsfájl**  hello tároló mappa **adfgetstarted**.

    Hello **meghatározása** szakaszban használt toospecify hello futásidejű beállításokat, lehet, toohello hive parancsfájl át Hive konfigurációs értékeket (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

    Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.

    Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.

   > [!NOTE]
   > "Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) hello példában használt JSON tulajdonságokat vonatkozó további információért.

2. Ellenőrizze, hogy látható-e hello **input.log** hello fájlban **adfgetstarted/inputdata** hello Azure blob Storage tárolóban, és futtassa a következő parancs toodeploy hello csővezeték hello mappájában. Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.

    ```PowerShell
    New-AzureRmDataFactoryPipeline $df -File .\MyFirstPipelinePSH.json
    ```
3. Gratulálunk, sikeresen létrehozta első folyamatát az Azure PowerShell használatával!

## <a name="monitor-pipeline"></a>Folyamat figyelése
Ezt a lépést használhatja az Azure PowerShell toomonitor egy az Azure data factory lesz.

1. Futtatás **Get-AzureRmDataFactory** , és rendelje hozzá a hello kimeneti tooa **$df** változó.

    ```PowerShell
    $df=Get-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name FirstDataFactoryPSH
    ```
2. Futtatás **Get-AzureRmDataFactorySlice** hello az összes szeletek tooget adatait **EmpSQLTable**, vagyis hello eredménytábla hello folyamatának.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```
    Figyelje meg, hogy az itt megadott StartDateTime hello azonos indítsa el a kérdéses hello adatcsatorna JSON hello. Itt egy hello minta kimenet:

    ```PowerShell
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : FirstDataFactoryPSH
    DatasetName       : AzureBlobOutput
    Start             : 7/1/2017 12:00:00 AM
    End               : 7/2/2017 12:00:00 AM
    RetryCount        : 0
    State             : InProgress
    SubState          :
    LatencyStatus     :
    LongRetryCount    : 0
    ```
3. Futtatás **Get-AzureRmDataFactoryRun** tooget hello részletek tevékenység egy adott szelet futtatása.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName AzureBlobOutput -StartDateTime 2017-07-01
    ```

    Itt egy hello minta kimenet: 

    ```PowerShell
    Id                  : 0f6334f2-d56c-4d48-b427-d4f0fb4ef883_635268096000000000_635292288000000000_AzureBlobOutput
    ResourceGroupName   : ADFTutorialResourceGroup
    DataFactoryName     : FirstDataFactoryPSH
    DatasetName         : AzureBlobOutput
    ProcessingStartTime : 12/18/2015 4:50:33 AM
    ProcessingEndTime   : 12/31/9999 11:59:59 PM
    PercentComplete     : 0
    DataSliceStart      : 7/1/2017 12:00:00 AM
    DataSliceEnd        : 7/2/2017 12:00:00 AM
    Status              : AllocatingResources
    Timestamp           : 12/18/2015 4:50:33 AM
    RetryAttempt        : 0
    Properties          : {}
    ErrorMessage        :
    ActivityName        : RunSampleHiveActivity
    PipelineName        : MyFirstPipeline
    Type                : Script
    ```
    Akkor is tartsa futtatja ezt a parancsmagot amíg megjelenik a hello szelet **készen** állapota vagy **sikertelen** állapotát. Ha hello szelet készen állapotban van, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.  Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig.

    ![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-powershell/three-ouptut-files.png)

> [!IMPORTANT]
> Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.
>
> hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént. Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.
>
>

## <a name="summary"></a>Összefoglalás
Ebben az oktatóanyagban az Azure data factory tooprocess adatok Hive parancsfájl futtatásával a HDInsight hadoop-fürthöz létrehozott. A lépéseket követve hello Azure portál toodo hello a Data Factory Editor hello használt:

1. Létrehozott egy Azure **data factoryt**.
2. Létrehozott két **társított szolgáltatást**:
   1. **Az Azure Storage** kapcsolódó szolgáltatás toolink az Azure blob storage, amely a bemeneti/kimeneti fájlok toohello adat-előállító tárolja.
   2. **Az Azure HDInsight** igény kapcsolódó szolgáltatás toolink egy igény szerinti HDInsight Hadoop fürthöz toohello adat-előállítóban. Az Azure Data Factory egy HDInsight Hadoop fürthöz just-in-time tooprocess bemeneti adatok és a által előállított kimeneti adatokat hoz létre.
3. Két létrehozott **adatkészletek**, leírják hello folyamat HDInsight Hive tevékenység bemeneti és kimeneti adatokat.
4. Létrehozott egy **folyamatot** egy **HDInsight Hive**-tevékenységgel.

## <a name="next-steps"></a>Következő lépések
Az oktatóanyag során létrehozott egy folyamatot egy adatátalakítási tevékenységgel (HDInsight-tevékenység), amely Hive-parancsfájlt futtat egy igény szerinti Azure HDInsight-fürtön. Hogyan toouse egy Azure Blob tooAzure SQL, a másolási tevékenység toocopy adatait: toosee [oktatóanyag: adatok másolása az Azure Blob tooAzure SQL](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

## <a name="see-also"></a>Lásd még:
| Témakör | Leírás |
|:--- |:--- |
| [A Data Factory parancsmagjainak leírása](/powershell/module/azurerm.datafactories) |A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentáció. |
| [Folyamatok](data-factory-create-pipelines.md) |Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás. |
