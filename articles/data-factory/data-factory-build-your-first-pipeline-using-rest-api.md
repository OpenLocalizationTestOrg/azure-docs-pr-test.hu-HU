---
title: "aaaBuild az első adat-előállítóban (REST) |} Microsoft Docs"
description: "Az oktatóanyag során egy minta Azure Data Factory-adatcsatornát fogunk létrehozni a Data Factory REST API-val."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 7e0a2465-2d85-4143-a4bb-42e03c273097
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 07/10/2017
ms.author: spelluru
ms.openlocfilehash: 5d8e39bd598cca35f91d501bad74e8a8436f8f89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-build-your-first-azure-data-factory-using-data-factory-rest-api"></a>Oktatóanyag: Az első data factory létrehozása a Data Factory REST API használatával
> [!div class="op_single_selector"]
> * [Áttekintés és előfeltételek](data-factory-build-your-first-pipeline.md)
> * [Azure Portal](data-factory-build-your-first-pipeline-using-editor.md)
> * [Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
> * [PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
> * [Resource Manager-sablon](data-factory-build-your-first-pipeline-using-arm.md)
> * [REST API](data-factory-build-your-first-pipeline-using-rest-api.md)
>
>


Ebben a cikkben az első az Azure data factory Data Factory REST API toocreate meg használja. toodo hello az oktatóanyagot más eszközök/SDK használatával hello legördülő listából válasszon hello lehetőségek közül.

Ebben az oktatóanyagban hello folyamat rendelkezik egy tevékenység: **HDInsight Hive tevékenység**. Ez a tevékenység hive parancsfájlok futtatására szolgál, hogy átalakítások bemeneti adatok tooproduce kimeneti adatok Azure HDInsight-fürtöt. hello csővezeték ütemezett toorun, a hónap közötti hello kezdő és záró időpontjának megadása után.

> [!NOTE]
> Ez a cikk nem foglalkozik az összes hello REST API-t. A REST API átfogó dokumentációjáért lásd: [A Data Factory REST API-jának leírása](/rest/api/datafactory/).
> 
> Egy folyamathoz több tevékenység is tartozhat. És láncolt telepítését (egymás után futtatni egy tevékenységet) két tevékenység által hello bemeneti hello az adatkészlet többi tevékenység hello kimeneti adatkészlet egy tevékenység beállítását. További tudnivalókért lásd: [Ütemezés és végrehajtás a Data Factoryban](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline).


## <a name="prerequisites"></a>Előfeltételek
* Olvassa végig [oktatóanyag – áttekintés](data-factory-build-your-first-pipeline.md) cikkben és a teljes hello **előfeltétel** lépéseket.
* Telepítse gépére a [Curl](https://curl.haxx.se/dlwiz/) eszközt. A további parancsok toocreate egy adat-előállító hello CURL eszközt kell használni.
* Az [ebben a cikkben](../azure-resource-manager/resource-group-create-service-principal-portal.md) szereplő utasításokat követve végezze el a következőket:
  1. Hozzon létre egy **ADFGetStartedApp** nevű webalkalmazást az Azure Active Directoryban.
  2. Szerezze be az **ügyfél-azonosítót** és a **titkos kulcsot**.
  3. Szerezze be a **bérlőazonosítót**.
  4. Rendelje hozzá a hello **ADFGetStartedApp** alkalmazás toohello **Data Factory közreműködői** szerepkör.
* Telepítse az [Azure PowerShellt](/powershell/azure/overview).
* Indítsa el **PowerShell** és futtatási hello a következő parancsot. Azure PowerShell nyitva hagyja az oktatóanyag hello végéig. Zárja be és nyissa meg újra, ha újra kell toorun hello parancsok.
  1. Futtatás **Login-AzureRmAccount** , és írja be a hello felhasználónevet és jelszót toosign használhatja a toohello Azure-portálon.
  2. Futtatás **Get-AzureRmSubscription** tooview összes hello előfizetések ehhez a fiókhoz.
  3. Futtatás **Get-AzureRmSubscription – SubscriptionName NameOfAzureSubscription |} Set-AzureRmContext** ki a toowork tooselect hello előfizetést. Cserélje le **NameOfAzureSubscription** hello nevet, az Azure-előfizetéshez.
* Hozzon létre egy Azure erőforráscsoport nevű **ADFTutorialResourceGroup** hello hello PowerShell parancsot a következő futtatásával:

    ```PowerShell
    New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"
    ```

   Egyes ebben az oktatóanyagban hello lépések azt feltételezik, hogy hello erőforráscsoportot ADFTutorialResourceGroup használatát. Ha egy másik erőforráscsoportban található használja, ebben az oktatóanyagban kell toouse hello ADFTutorialResourceGroup helyett az erőforráscsoport nevét.

## <a name="create-json-definitions"></a>JSON-definíciók létrehozása
Következő hello mappáját curl.exe a JSON-fájlok létrehozása

### <a name="datafactoryjson"></a>datafactory.json
> [!IMPORTANT]
> Egyedinek kell lennie globálisan, ezért érdemes tooprefix/utótag ADFCopyTutorialDF toomake azt egy egyedi nevet.
>
>

```JSON
{
    "name": "FirstDataFactoryREST",
    "location": "WestUS"
}
```

### <a name="azurestoragelinkedservicejson"></a>azurestoragelinkedservice.json
> [!IMPORTANT]
> Az **accountname** és az **accountkey** kifejezés helyére írja be Azure Storage-tárfiókja nevére, illetve kulcsát. toolearn hogyan tooget tárhelyét a hozzáférési kulcs, hogyan tooview, másolása és újragenerálása tárolási hívóbetűk a hello adatainak megjelenítéséhez [a tárfiók kezelése](../storage/common/storage-create-storage-account.md#manage-your-storage-account).
>
>

```JSON
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
    }
}
```

### <a name="hdinsightondemandlinkedservicejson"></a>hdinsightondemandlinkedservice.json

```JSON
{
    "name": "HDInsightOnDemandLinkedService",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "linkedServiceName": "AzureStorageLinkedService"
        }
    }
}
```

hello következő táblázat ismerteti hello részlet használt hello JSON-tulajdonságok:

| Tulajdonság | Leírás |
|:--- |:--- |
| ClusterSize |Hello HDInsight-fürt méretét. |
| TimeToLive |Adott hello hello HDInsight-fürtjéhez, üresjárati idejét határozza meg, törlés előtt. |
| linkedServiceName |Adja meg, amely a HDInsight által létrehozott használt toostore hello naplók hello storage-fiók |

Vegye figyelembe a következő pontok hello:

* hello adat-előállító létrehoz egy **Linux-alapú** a fenti JSON hello meg HDInsight-fürthöz. További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).
* Igény szerinti HDInsight-fürt helyett **saját HDInsight-fürtöt** is használhat. További információ: [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (HDInsight társított szolgáltatás).
* hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet feldolgozása, kivéve, ha egy meglévő élő fürthöz (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.

    Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

További információkért lásd: [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).

### <a name="inputdatasetjson"></a>inputdataset.json

```JSON
{
    "name": "AzureBlobInput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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
| columnDelimiter |oszlopok hello naplófájlokban határolja vesszővel (,) karakter |
| frequency/interval |gyakoriságának beállítása tooMonth és időköz 1, ami azt jelenti, hogy hello bemeneti szeletek elérhető havi. |
| external |Ez a tulajdonság beállítása tootrue hello bemeneti adatok nem generálja hello Data Factory szolgáltatásnak. |

### <a name="outputdatasetjson"></a>outputdataset.json

```JSON
{
    "name": "AzureBlobOutput",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "AzureStorageLinkedService",
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

### <a name="pipelinejson"></a>pipeline.json
> [!IMPORTANT]
> Cserélje a **storageaccountname** kifejezést Azure Storage-fiókja nevére.
>
>

```JSON
{
    "name": "MyFirstPipeline",
    "properties": {
        "description": "My first Azure Data Factory pipeline",
        "activities": [{
            "type": "HDInsightHive",
            "typeProperties": {
                "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                "scriptLinkedService": "AzureStorageLinkedService",
                "defines": {
                    "inputtable": "wasb://adfgetstarted@<stroageaccountname>.blob.core.windows.net/inputdata",
                    "partitionedtable": "wasb://adfgetstarted@<stroageaccountname>t.blob.core.windows.net/partitioneddata"
                }
            },
            "inputs": [{
                "name": "AzureBlobInput"
            }],
            "outputs": [{
                "name": "AzureBlobOutput"
            }],
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
        }],
        "start": "2017-07-10T00:00:00Z",
        "end": "2017-07-11T00:00:00Z",
        "isPaused": false
    }
}
```

Hello JSON részlet egy folyamatot, amely tartalmaz egy adott tevékenység által használt Hive tooprocess adatok egy HDInsight-fürt létrehozásához.

hello Hive parancsfájl, **partitionweblogs.hql**, hello Azure storage-fiók tárolva van (hello scriptLinkedService nevű által megadott **StorageLinkedService**), majd a **parancsfájl**  hello tároló mappa **adfgetstarted**.

Hello **meghatározása** szakasz Hive értékként toohello hive parancsfájl átadott futásidejű beállításokat határoz meg, (például ${hiveconf: inputtable}, {hiveconf:partitionedtable} $).

Hello **start** és **end** hello folyamatának tulajdonságok hello hello adatcsatorna aktív időszakát határozza meg.

Hello tevékenység JSON-NÁ, meghatározza, hogy hello Hive parancsfájl futó hello által megadott hello számítási **linkedServiceName** – **HDInsightOnDemandLinkedService**.

> [!NOTE]
> "Adatcsatorna JSON" című [folyamatok és az Azure Data Factory tevékenységek](data-factory-create-pipelines.md) használt a fenti példa hello JSON-tulajdonságok vonatkozó további információért.
>
>

## <a name="set-global-variables"></a>Globális változók beállítása
Az Azure PowerShell-lel hajtható végre hello parancsok után hello értékeket a saját cserélje le a következő:

> [!IMPORTANT]
> Az ügyfél-azonosító, a titkos ügyfélkód, a bérlőazonosító és az előfizetés-azonosító beszerzésével kapcsolatban olvassa el az [Előfeltételek](#prerequisites) című fejezetet.
>
>

```PowerShell
$client_id = "<client ID of application in AAD>"
$client_secret = "<client key of application in AAD>"
$tenant = "<Azure tenant ID>";
$subscription_id="<Azure subscription ID>";

$rg = "ADFTutorialResourceGroup"
$adf = "FirstDataFactoryREST"
```


## <a name="authenticate-with-aad"></a>Hitelesítés az AAD segítségével

```PowerShell
$cmd = { .\curl.exe -X POST https://login.microsoftonline.com/$tenant/oauth2/token  -F grant_type=client_credentials  -F resource=https://management.core.windows.net/ -F client_id=$client_id -F client_secret=$client_secret };
$responseToken = Invoke-Command -scriptblock $cmd;
$accessToken = (ConvertFrom-Json $responseToken).access_token;

(ConvertFrom-Json $responseToken)
```


## <a name="create-data-factory"></a>Data factory létrehozása
Ebben a lépésben egy **FirstDataFactoryREST** nevű Azure-adatelőállítót fog létrehozni. A data factory egy vagy több folyamattal rendelkezhet. A folyamaton belül egy vagy több tevékenység lehet. Például a másolási tevékenység toocopy adatait a forrás tooa cél tárolóban és a HDInsight Hive tevékenység toorun a Hive parancsfájl tootransform adatokat. Futtassa a következő parancsok toocreate hello adat-előállító hello:

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    Győződjön meg arról, hogy az itt megadott (ADFCopyTutorialDF) egyező hello hello megadott név hello adat-előállító hello neve **datafactory.json**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@datafactory.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/FirstDataFactoryREST?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adat-előállító létrehozása sikeresen befejeződött, megjelenik az hello data factory hello a JSON hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```

Vegye figyelembe a következő pontok hello:

* Azure Data Factory hello hello nevének globálisan egyedi kell lennie. Ha az eredmények hello hibát látja: **nem érhető el adat-előállító "FirstDataFactoryREST"**, hello a következő lépéseket:
  1. Hello nevének módosítása (például yournameFirstDataFactoryREST) a hello **datafactory.json** fájlt. A Data Factory-összetevők elnevezési szabályait a [Data Factory - Naming Rules](data-factory-naming-rules.md) (Data Factory – Elnevezési szabályok) című témakörben találhatja.
  2. Az első parancs hello ahol hello **$cmd** változó értéket kapja, cserélje le a FirstDataFactoryREST hello új névvel és hello parancsot.
  3. Futtassa a hello következő két parancsok tooinvoke hello REST API toocreate hello data factory és nyomtatás hello hello művelet eredménye.
* toocreate adat-előállító esetekben kell toobe egy Azure-előfizetés hello közreműködői/rendszergazda
* hello adat-előállító nevét hello előfordulhat, hogy a jövőbeli hello DNS-névként regisztrált, és így nyilvánosan láthatóvá válhat.
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

Mielőtt létrehozna egy folyamatot, kell toocreate néhány adat-előállító entitások először. Először létre kell hoznia összekapcsolt szolgáltatások toolink adatok tárolók/kiszámítja tooyour adatok tárolásához, adja meg a bemeneti és kimeneti adatkészletek toorepresent adatai a csatolt adattárolókhoz.

## <a name="create-linked-services"></a>Társított szolgáltatások létrehozása
Ebben a lépésben csatolja a az Azure Storage-fiók és az igény szerinti Azure HDInsight fürt tooyour adat-előállítóban. hello tartás hello hello adatcsatorna Ez a példa a bemeneti és kimeneti adatok Azure Storage-fiók. HDInsight kapcsolódó szolgáltatás hello használt toorun hello folyamatának Ez a példa hello tevékenységben megadott Hive parancsfájl.

### <a name="create-azure-storage-linked-service"></a>Azure Storage társított szolgáltatás létrehozása
Ebben a lépésben az Azure Storage-fiók tooyour adat-előállító hivatkozásra. Ebben az oktatóanyagban a hello használhatja ugyanazon Azure Storage-fiók toostore bemeneti/kimeneti adatok és hello HQL parancsfájlt.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data “@azurestoragelinkedservice.json” https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/AzureStorageLinkedService?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-azure-hdinsight-linked-service"></a>Azure HDInsight társított szolgáltatás létrehozása
Ebben a lépésben egy igény szerinti HDInsight fürt tooyour adat-előállító hivatkozásra. az automatikusan létrehozott futásidőben és törlése után feldolgozásra és üresjárati hello megadott időtartamig hello HDInsight-fürthöz. Igény szerinti HDInsight-fürt helyett saját HDInsight-fürtöt is használhat. További információ: [Compute Linked Services](data-factory-compute-linked-services.md) (Számítási társított szolgáltatás).

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@hdinsightondemandlinkedservice.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/linkedservices/hdinsightondemandlinkedservice?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello társított szolgáltatás létrehozása sikeresen befejeződött úgy, hogy hello JSON hello szolgáltatáshoz kapcsolódó hello **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-datasets"></a>Adatkészletek létrehozása
Ebben a lépésben létrehozni adatkészletek toorepresent hello bemeneti és kimeneti adatai Hive feldolgozásra. Ezek az adatkészletek tekintse meg a toohello **StorageLinkedService** Ez az oktatóanyag korábbi részében létrehozott. hello társított szolgáltatás pontok tooan Azure Storage-fiókot, és adja meg tároló, mappa, fájl neve adatkészletek hello tárolóban, amely tárolja a bemeneti és kimeneti adatokat.

### <a name="create-input-dataset"></a>Bemeneti adatkészlet létrehozása
Ebben a lépésben hello bemeneti adatkészlet toorepresent bemeneti adatok hello Azure Blob storage-ban tárolt hoz létre.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@inputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobInput?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```

### <a name="create-output-dataset"></a>Kimeneti adatkészlet létrehozása
Ebben a lépésben hello kimeneti adatkészlet toorepresent kimeneti tárolt adatok hello Azure Blob Storage tárolóban hoz létre.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@outputdataset.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/AzureBlobOutput?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```

## <a name="create-pipeline"></a>Folyamat létrehozása
Ebben a lépésben létrehozza a **HDInsightHive** tevékenységgel rendelkező első adatcsatornát. Bemeneti szelet érhető havonta (gyakoriság: hónap, időköz: 1), a kimeneti szelet havonta jön létre, és hello Feladatütemező hello tevékenység is tulajdonsága toomonthly. hello kimeneti adatkészlet és hello tevékenység Feladatütemező hello beállításait meg kell egyeznie. Kimeneti adatkészlet jelenleg milyen meghajtók hello ütemezését, így még akkor is, ha hello tevékenység nem ad kimenetet kell létrehoznia egy kimeneti adatkészletet. Ha hello tevékenység egyetlen bemeneti nem veszi, kihagyhatja létrehozása hello bemeneti adatkészletet.

Ellenőrizze, hogy látható-e hello **input.log** hello fájlban **adfgetstarted/inputdata** hello Azure blob Storage tárolóban, és futtassa a következő parancs toodeploy hello csővezeték hello mappájában. Hello óta **start** és **end** az elmúlt hello beállítása és **isPaused** van a (hello feldolgozási soros tevékenységek) set toofalse, hello csővezeték azonnal telepítése után.

1. Rendelje hozzá a hello parancs toovariable nevű **cmd**.

    ```PowerShell
    $cmd = {.\curl.exe -X PUT -H "Authorization: Bearer $accessToken" -H "Content-Type: application/json" --data "@pipeline.json" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datapipelines/MyFirstPipeline?api-version=2015-10-01};
    ```
2. Hello parancs futtatásával **Invoke-Command**.

    ```PowerShell
    $results = Invoke-Command -scriptblock $cmd;
    ```
3. Hello eredményeinek megtekintése. Ha hello adatkészlet sikeresen létrejött, a hello hello adatkészlet JSON hello látható **eredmények**; ellenkező esetben egy hibaüzenet jelenik meg.

    ```PowerShell
    Write-Host $results
    ```
4. Gratulálunk, sikeresen létrehozta első folyamatát az Azure PowerShell használatával!

## <a name="monitor-pipeline"></a>Folyamat figyelése
Ebben a lépésben a Data Factory REST API toomonitor szeletek hello folyamat alatt előállított funkcióval.

```PowerShell
$ds ="AzureBlobOutput"

$cmd = {.\curl.exe -X GET -H "Authorization: Bearer $accessToken" https://management.azure.com/subscriptions/$subscription_id/resourcegroups/$rg/providers/Microsoft.DataFactory/datafactories/$adf/datasets/$ds/slices?start=1970-01-01T00%3a00%3a00.0000000Z"&"end=2016-08-12T00%3a00%3a00.0000000Z"&"api-version=2015-10-01};

$results2 = Invoke-Command -scriptblock $cmd;

IF ((ConvertFrom-Json $results2).value -ne $NULL) {
    ConvertFrom-Json $results2 | Select-Object -Expand value | Format-Table
} else {
        (convertFrom-Json $results2).RemoteException
}
```

> [!IMPORTANT]
> Az igény szerinti HDInsight-fürt létrehozása általában eltart egy ideig (körülbelül 20 percig). Ezért várt hello csővezeték tootake **körülbelül 30 percet** tooprocess hello szelet.
>
>

Futtatás hello Invoke-Command és hello egy amíg megjelenik a hello szelet **készen** állapota vagy **sikertelen** állapota. Ha hello szelet készen állapotban van, ellenőrizze a hello **partitioneddata** hello mappájában **adfgetstarted** a blob-tároló hello tárolóhoz kimeneti adatokat.  igény szerinti HDInsight-fürtök létrehozása hello általában némi időt vesz igénybe.

![kimeneti adatok](./media/data-factory-build-your-first-pipeline-using-rest-api/three-ouptut-files.png)

> [!IMPORTANT]
> hello bemeneti fájl törlése hello szelet feldolgozása sikeresen megtörtént. Ezért ha szeretné, hogy toorerun hello szelet, vagy újra hello oktatóanyag, feltöltési hello bemeneti fájl (input.log) toohello inputdata mappa hello adfgetstarted tároló.
>
>

Használja az Azure portál toomonitor szeletek is, és hárítsa el. További információk: [Monitor pipelines using Azure portal](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) (Adatcsatornák figyelése az Azure Portal használatával).

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
| [Data Factory REST API referenciája](/rest/api/datafactory/) |A Data Factory-parancsmagokkal kapcsolatos átfogó dokumentáció. |
| [Folyamatok](data-factory-create-pipelines.md) |Ez a cikk segít megérteni a folyamatok és az Azure Data Factory tevékenységeket, és hogyan toouse tooconstruct végpont adatvezérelt munkafolyamatok a forgatókönyv vagy üzleti őket. |
| [Adatkészletek](data-factory-create-datasets.md) |Ennek a cikknek a segítségével megismerheti az adatkészleteket az Azure Data Factoryban. |
| [Ütemezés és végrehajtás](data-factory-scheduling-and-execution.md) |Ez a cikk ismerteti az Azure Data Factory alkalmazásmodell hello ütemezés és a végrehajtási szempontjait. |
| [Folyamatok figyelése és felügyelete a Monitoring App használatával](data-factory-monitor-manage-app.md) |Ez a cikk ismerteti, hogyan toomonitor, kezelése és hibakeresése folyamatok használatával hello figyelés & a felügyeleti alkalmazás. |
