---
title: "igény szerinti Hadoop-fürtök használata a Data Factory - Azure HDInsight aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate igény szerinti Hadoop-fürtök a HDInsight az Azure Data Factory használatával."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: spelluru
manager: jhubbard
editor: cgronlun
ms.assetid: 1f3b3a78-4d16-4d99-ba6e-06f7bb185d6a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/20/2017
ms.author: spelluru
ms.openlocfilehash: c869776ac270e37dec710b5fc8d2a792d9263129
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-on-demand-hadoop-clusters-in-hdinsight-using-azure-data-factory"></a>Igény szerinti Hadoop-fürtök létrehozása a Hdinsightban Azure Data Factory használatával
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

[Az Azure Data Factory](../data-factory/data-factory-introduction.md) egy felhőalapú integrációs szolgáltatás koordinálja és hello mozgás és adatok átalakítása automatizálja. Hozzon létre egy HDInsight Hadoop fürthöz just-in-time tooprocess egy bemeneti adatszelet képes és hello fürt törlése, ha hello feldolgozása befejeződött. Egy igény szerinti HDInsight Hadoop-fürt használatának hello előnyei a következők:

- Ön egyetlen fizetési hello idő feladat fut hello HDInsight Hadoop cluster (és egy rövid konfigurálható üresjárati idő). a HDInsight-fürtök hello számlázás pro-értékelés percenként történik, függetlenül attól, hogy azokat, vagy nem. Egy igény szerinti HDInsight kapcsolódó szolgáltatás használatakor a Data Factory hello fürtök igény jönnek létre. És hello fürtök automatikusan törlődnek hello feladatok elvégzésekor. Ezért csak kell fizetnie hello feladat fut, és hello rövid üresjárati idő (live idő beállítása).
- Használatával a Data Factory-folyamathoz Munkafolyamat létrehozásához. Például lehet hello adatcsatorna toocopy adatait egy helyi SQL Server tooan Azure blob-tároló, a folyamat hello adatok az igény szerinti HDInsight Hadoop-fürt a Hive parancsfájlok és a Pig-parancsprogram futtatásával. Ezután másolja hello eredmény adatok tooan Azure SQL Data Warehouse BI alkalmazások tooconsume.
- Ütemezheti az hello munkafolyamat toorun időnként (óránként, naponta, hetente, havonta, stb.).

Az Azure Data Factoryben adat-előállító rendelkezhet egy vagy több adat folyamatok. Adatok folyamat rendelkezik egy vagy több tevékenységet. Tevékenységek két típusa van: [adatok mozgása tevékenységek](../data-factory/data-factory-data-movement-activities.md) és [adatok átalakítása tevékenységek](../data-factory/data-factory-data-transformation-activities.md). Adatok (jelenleg csak másolási tevékenység) tevékenységek toomove adat egy forrás adatokat tároló tooa cél adattárból használhatja. Adatok átalakítása tevékenységek tootransform/folyamat adatokat használ. HDInsight Hive tevékenység a Data Factory által támogatott hello átalakítása tevékenységek egyike. Ebben az oktatóanyagban hello Hive átalakítási tevékenységet használja.

Konfigurálhatja a hive tevékenység toouse saját HDInsight Hadoop-fürt vagy egy igény szerinti HDInsight Hadoop-fürt. Ebben az oktatóanyagban hello hello data factory-folyamathoz Hive tevékenység konfigurált toouse igény szerinti HDInsight-fürtöt. Ezért amikor hello tevékenység fut tooprocess egy adatszelet, ez történik:

1. HDInsight Hadoop-fürthöz, közvetlenül az időponthoz kötött tooprocess hello szelet automatikusan létrejön.  
2. hello bemeneti adatokat dolgozza fel a HiveQL-parancsfájlt hello fürtben futó.
3. HDInsight Hadoop-fürt hello hello feldolgozás befejezése után, hello fürt üresjáratban konfigurálva hello időn (timeToLive-beállítást) törlődik. A timeToLive üresjárati idő feldolgozásra hello következő adatszelet érhető el, hello ugyanabban a fürtben esetén használt tooprocess hello szelet.  

Ebben az oktatóanyagban hello hello hive tevékenységhez társított HiveQL-parancsfájlt hello a következő műveleteket hajtja végre:

1. Létrehoz egy külső táblát, amely a hivatkozások hello egy Azure Blob storage-ban tárolt nyers webes naplóadatokat.
2. Partíciók hello nyers adatok hónap és év szerint.
3. Tárolók hello particionált adatok hello Azure blob Storage tárolóban.

Ebben az oktatóanyagban hello hello hive tevékenységhez társított HiveQL-parancsfájlt, amely hivatkozások hello hello Azure Blob Storage tárolóban tárolt nyers webes naplóadatokat külső táblát hoz létre. Az alábbiakban hello minta sorok havonta hello bemeneti fájl.

```
2014-01-01,02:01:09,SAMPLEWEBSITE,GET,/blogposts/mvc4/step2.png,X-ARR-LOG-ID=2ec4b8ad-3cf0-4442-93ab-837317ece6a1,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,53175,871
2014-02-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
2014-03-01,02:01:10,SAMPLEWEBSITE,GET,/blogposts/mvc4/step7.png,X-ARR-LOG-ID=d7472a26-431a-4a4d-99eb-c7b4fda2cf4c,80,-,1.54.23.196,Mozilla/5.0+(Windows+NT+6.3;+WOW64)+AppleWebKit/537.36+(KHTML,+like+Gecko)+Chrome/31.0.1650.63+Safari/537.36,-,http://weblogs.asp.net/sample/archive/2007/12/09/asp-net-mvc-framework-part-4-handling-form-edit-and-post-scenarios.aspx,\N,200,0,0,30184,871
```

hello HiveQL parancsfájl partíciók hello nyers adatok hónap és év szerint. Három kimeneti mappa hello előző bemeneti alapján hoz létre. Minden mappa minden hónap bejegyzéseinek fájlt tartalmaz.

```
adfgetstarted/partitioneddata/year=2014/month=1/000000_0
adfgetstarted/partitioneddata/year=2014/month=2/000000_0
adfgetstarted/partitioneddata/year=2014/month=3/000000_0
```

Adat-előállító átalakítása tevékenységek hozzáadása tooHive tevékenységben listájáért lásd: [alakít át és elemez az Azure Data Factory használatával](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> Jelenleg csak létrehozhat HDInsight fürt 3.2-es verziójú Azure Data Factory.

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk utasításait hello megkezdése előtt a következő elemek hello kell rendelkeznie:

* [Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Azure PowerShell.

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell.md)]

### <a name="prepare-storage-account"></a>Készítse elő a storage-fiók
Ebben a forgatókönyvben toothree storage-fiók mentése használhatja:

- alapértelmezett tárfiók hello HDInsight-fürthöz
- a bemeneti adatok hello Storage-fiók
- tárfiók hello kimeneti adatai

toosimplify hello oktatóanyagban egy tárolási fiók tooserve hello három célra használja. hello ebben a szakaszban található Azure PowerShell-parancsfájlpélda hello a következő feladatokat hajtja végre:

1. Jelentkezzen be tooAzure.
2. Azure-erőforráscsoport létrehozása
3. Hozzon létre egy Azure Storage-fiókot.
4. Egy Blob-tároló hello storage-fiók létrehozása
5. Másolja a következő két fájlok toohello Blob tároló hello:

   * A bemeneti adatfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log)
   * HiveQL-parancsfájlt: [https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql](https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql)

     Mindkét fájljai a következő nyilvános blobtárolóban.


**tooprepare hello tárolási, és másolja hello fájlok Azure PowerShell használatával:**
> [!IMPORTANT]
> Adjon meg nevet hello Azure erőforráscsoport és hello parancsfájl által létrehozott hello Azure storage-fiók.
> Írja le **erőforráscsoport-név**, **tárfióknév**, és **tárfiók kulcsa** outputted hello parancsfájl. Már szükség hello a következő szakaszban.

```powershell
$resourceGroupName = "<Azure Resource Group Name>"
$storageAccountName = "<Azure Storage Account Name>"
$location = "East US 2"

$sourceStorageAccountName = "hditutorialdata"  
$sourceContainerName = "adfhiveactivity"

$destStorageAccountName = $storageAccountName
$destContainerName = "adfgetstarted" # don't change this value.

####################################
# Connect tooAzure
####################################
#region - Connect tooAzure subscription
Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
try{Get-AzureRmContext}
catch{Login-AzureRmAccount}
#endregion

####################################
# Create a resource group, storage, and container
####################################

#region - create Azure resources
Write-Host "`nCreating resource group, storage account and blob container ..." -ForegroundColor Green

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmStorageAccount `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName `
    -type Standard_LRS `
    -Location $location

$destStorageAccountKey = (Get-AzureRmStorageAccountKey `
    -ResourceGroupName $resourceGroupName `
    -Name $destStorageAccountName)[0].Value

$sourceContext = New-AzureStorageContext `
    -StorageAccountName $sourceStorageAccountName `
    -Anonymous
$destContext = New-AzureStorageContext `
    -StorageAccountName $destStorageAccountName `
    -StorageAccountKey $destStorageAccountKey

New-AzureStorageContainer -Name $destContainerName -Context $destContext
#endregion

####################################
# Copy files
####################################
#region - copy files
Write-Host "`nCopying files ..." -ForegroundColor Green

$blobs = Get-AzureStorageBlob `
    -Context $sourceContext `
    -Container $sourceContainerName

$blobs|Start-AzureStorageBlobCopy `
    -DestContext $destContext `
    -DestContainer $destContainerName

Write-Host "`nCopied files ..." -ForegroundColor Green
Get-AzureStorageBlob -Context $destContext -Container $destContainerName
#endregion

Write-host "`nYou will use hello following values:" -ForegroundColor Green
write-host "`nResource group name: $resourceGroupName"
Write-host "Storage Account Name: $destStorageAccountName"
write-host "Storage Account Key: $destStorageAccountKey"

Write-host "`nScript completed" -ForegroundColor Green
```

Ha a PowerShell-parancsfájllal hello segítségre van szüksége, tekintse meg [Using hello az Azure Storage Azure PowerShell](../storage/common/storage-powershell-guide-full.md). Ha például az Azure parancssori felület toouse ehelyett látható hello [függelék](#appendix) szakasz hello Azure CLI-parancsfájlt.

**tooexamine hello tárolási fiók és hello tartalma**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **erőforráscsoportok** hello bal oldali ablaktáblán.
3. Kattintson duplán az erőforráscsoport neve hello a PowerShell parancsfájl létrehozott. Hello szűrőt használhat, ha túl sok erőforrás-csoportok szerepel a listában.
4. A hello **erőforrások** csempe, kell rendelkeznie kivéve, ha hello erőforráscsoport megosztása más projektek felsorolt egy erőforrás. Adott erőforrás hello tárfiók a korábban meghatározott hello nevű. Kattintson a hello tárfiók neve.
5. Kattintson a hello **Blobok** csempék.
6. Kattintson a hello **adfgetstarted** tároló. Két mappák látja: **inputdata** és **parancsfájl**.
7. Nyissa meg a hello mappa, és ellenőrizze a hello mappákban hello fájlok. hello inputdata tartalmaz hello input.log fájl bemeneti adatokkal, és hello parancsfájlmappa hello HiveQL-parancsfájlt tartalmazza.

## <a name="create-a-data-factory-using-resource-manager-template"></a>A Resource Manager sablonnal adat-előállító létrehozása
Hello tárfiókot, hello bemeneti adatok és hello előkészített HiveQL-parancsfájlt készen áll a toocreate egy az Azure data factory áll. Többféleképpen az adat-előállító létrehozása. Ebben az oktatóanyagban létrehoz egy adat-előállító hello Azure-portál használatával egy Azure Resource Manager-sablon üzembe helyezésével. A Resource Manager-sablon használatával is telepítheti [Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) és [Azure PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template). Más data factory létrehozási módszert, lásd: [oktatóanyag: az első adat-előállító létrehozása](../data-factory/data-factory-build-your-first-pipeline.md).

1. Kattintson a következő kép toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello. hello sablon https://hditutorialdata.blob.core.windows.net/adfhiveactivity/data-factory-hdinsight-on-demand.json helyezkedik el. Lásd: hello [adat-előállító entitások hello sablonban](#data-factory-entities-in-the-template) szakasz hello sablon entitásokból kapcsolatos részletes információkat. 

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Fadfhiveactivity%2Fdata-factory-hdinsight-on-demand.json" target="_blank"><img src="./media/hdinsight-hadoop-create-linux-clusters-adf/deploy-to-azure.png" alt="Deploy tooAzure"></a>
2. Válassza ki **használata meglévő** hello beállítása **erőforráscsoport** beállítás, és válassza hello neve (a PowerShell-parancsfájl használatával) hello előző lépésben létrehozott hello erőforráscsoportot.
3. Adjon meg egy nevet az hello data factory (**adat-előállító**). Ez a név globálisan egyedinek kell lennie.
4. Adja meg a hello **tárfióknév** és **tárfiók kulcsa** hello előző lépésben megírt.
5. Válassza ki **toohello feltételek és kikötések elfogadom** elolvasása után említettük **feltételek és kikötések**.
6. Válassza ki **PIN-kód toodashboard** lehetőséget.
6. Kattintson a **beszerzés/létrehozása**. Megtekintheti a csempe irányítópult nevű hello **telepítése sablon-üzembehelyezés**. Várjon, amíg hello **erőforráscsoport** az erőforráscsoport panel nyílik meg. Hello csempe jelenik meg, az erőforráscsoport neve tooopen hello erőforrás csoport panel is kattinthat.
6. Ha hello erőforráscsoport panel még nincs megnyitva, kattintson a hello csempe tooopen hello erőforráscsoportot. Most megjelenik egy további data factory erőforrás továbbá felsorolt toohello tárolási fiók erőforrás.
7. Kattintson a data factory hello nevére (hello megadott érték **adat-előállító** paraméter).
8. Hello adat-előállító paneljén kattintson hello **Diagram** csempére. hello ábrán egy bemeneti adatkészlet, és egy kimeneti adatkészlet egy tevékenységet:

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat diagramja](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-pipeline-diagram.png)

    hello nevek hello Resource Manager-sablon vannak definiálva.
9. Kattintson duplán a **AzureBlobOutput**.
10. A hello **Recent frissítése szeletek**, ekkor megjelenik egy szelet. Ha hello állapota **folyamatban**, várja meg, amíg nem módosítják túl**készen**. Általában vesz igénybe **20 perc** toocreate HDInsight-fürtöt.

### <a name="check-hello-data-factory-output"></a>Hello data factory kimeneti ellenőrzése

1. Használja az azonos hello hello utolsó munkamenet toocheck hello tárolók hello adfgetstarted tároló eljárását. Nincsenek két új tárolók továbbá túl**adfgetsarted**:

   * Egy tároló hello mintája nevű: `adf<yourdatafactoryname>-linkedservicename-datetimestamp`. Ez a tároló egy hello alapértelmezett tároló hello HDInsight-fürthöz.
   * adfjobs: Ebben a tárolóban hello ADF feladatnaplóit hello tárolója.

     hello data factory kimeneti tárolódik **afgetstarted** hello Resource Manager sablon konfigurált.
2. Kattintson a **adfgetstarted**.
3. Kattintson duplán a **partitioneddata**. Megjelenik egy **év = 2014** mappa mert összes hello webes naplókat dátuma 2014-ben.

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-year.png)

    Ha Ön is részletekbe menően tárhatják hello lista, január, február és március kell tekintse meg a három mappa. És minden hónapban van egy naplófájl.

    ![Az Azure Data Factory HDInsight igény Hive tevékenység folyamat kimeneti](./media/hdinsight-hadoop-create-linux-clusters-adf/hdinsight-adf-output-month.png)

## <a name="data-factory-entities-in-hello-template"></a>Data Factory entitások hello sablonban
Itt látható, hogyan néz hello legfelső szintű erőforrás-kezelő sablon egy adat-előállító:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

### <a name="define-data-factory"></a>Data Factory definiálása
Megadhat egy adat-előállító hello Resource Manager-sablon látható módon a következő minta hello:  

```json
"resources": [
{
    "name": "[parameters('dataFactoryName')]",
    "apiVersion": "[variables('apiVersion')]",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "westus",
}
```
hello dataFactoryName hello adat-előállító hello sablon telepítésekor megadott hello neve. Adat-előállító jelenleg csak a hello USA keleti régiója, USA nyugati régiója és Észak-Európa következő régiókban támogatott.

### <a name="defining-entities-within-hello-data-factory"></a>Hello adat-előállító belüli definiálása
hello következő adat-előállító entitások definiált hello JSON-sablon:

* [Azure Storage társított szolgáltatás](#azure-storage-linked-service)
* [HDInsight igény szerinti társított szolgáltatás](#hdinsight-on-demand-linked-service)
* [Azure blobbemeneti adatkészlet](#azure-blob-input-dataset)
* [Azure blobkimeneti adatkészlet](#azure-blob-output-dataset)
* [Másolási tevékenységgel rendelkező adatfolyamat](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Azure Storage társított szolgáltatás
hello Azure Storage társított szolgáltatás hivatkozások a az Azure storage-fiók toohello adat-előállítóban. Ebben az oktatóanyagban hello ugyanabban a tárfiókban lesz hello alapértelmezett HDInsight tárfiókot, a bemeneti adatokat tároló és a kimeneti adatok tárolására. Ezért adja meg csak egy Azure Storage társított szolgáltatásnak. A hello társított szolgáltatás definíciójának akkor adja meg a hello és az Azure storage-fiók kulcsát. Lásd: [Azure Storage társított szolgáltatásnak](../data-factory/data-factory-azure-blob-connector.md#azure-storage-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért egy Azure Storage társított szolgáltatásnak.

```json
{
    "name": "[variables('storageLinkedServiceName')]",
    "type": "linkedservices",
    "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```
Hello **connectionString** használ hello storageAccountName és storageAccountKey paraméterek. Ezek a paraméterek értékeit hello sablon telepítése során adja meg.  

#### <a name="hdinsight-on-demand-linked-service"></a>HDInsight igény szerinti társított szolgáltatás
A hello igény szerinti HDInsight társított szolgáltatás definíciójának, megadhatja a hello adat-előállító szolgáltatás toocreate egy HDInsight Hadoop futásidőben fürt által használt konfigurációs paraméterek értékét. Lásd: [összekapcsolt szolgáltatások számítási](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) JSON használt tulajdonságok toodefine vonatkozó további információért cikk egy HDInsight igény társított szolgáltatást.  

```json

{
    "type": "linkedservices",
    "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "HDInsightOnDemand",
        "typeProperties": {
            "version": "3.5",
            "clusterSize": 1,
            "timeToLive": "00:05:00",
            "osType": "Linux",
            "sshUserName": "myuser",                            
            "sshPassword": "MyPassword!",
            "linkedServiceName": "[variables('storageLinkedServiceName')]"
        }
    }
}
```
Vegye figyelembe a következő pontok hello:

* hello adat-előállító létrehoz egy **Linux-alapú** meg HDInsight-fürthöz.
* hello HDInsight Hadoop-fürt jön létre hello azonos hello tárfiók és a régióban.
* Értesítés hello *timeToLive* beállítást. hello adat-előállító hello fürt üresjárati folyamatban van 30 perc után automatikusan törli hello fürt.
* hello HDInsight-fürtöt hoz létre egy **alapértelmezett tároló** az Ön által megadott hello JSON hello blob storage (**linkedServiceName**). HDInsight nem törli a tárolót hello fürt törlésekor. Ez a működésmód szándékos. Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén a HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz toobe (**timeToLive**), és törlődik, amikor hello feldolgozási hajtja végre.

További információkért lásd: [On-demand HDInsight Linked Service](../data-factory/data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Igény szerinti HDInsight társított szolgáltatás).

> [!IMPORTANT]
> Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: "adf**yourdatafactoryname**-**linkedservicename**- datetimestamp". Használjon például az eszközök [Microsoft Tártallózó](http://storageexplorer.com/) toodelete tárolókat az az Azure blob-tároló.

#### <a name="azure-blob-input-dataset"></a>Azure blobbemeneti adatkészlet
Hello bemeneti adatkészlet-definícióban blob tároló, mappa, és hello bemeneti adatokat tartalmazó fájlt hello nevét kell megadni. Lásd: [Azure-Blob adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.

```json

{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
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

Figyelje meg a következő hello JSON-definícióban meghatározott beállításokat hello:

```json
"fileName": "input.log",
"folderPath": "adfgetstarted/inputdata",
```

#### <a name="azure-blob-output-dataset"></a>Azure Blob kimeneti adatkészlet
Hello kimeneti adatkészlet-definícióban és a blob tároló hello kimeneti adatokat tartalmazó mappa hello nevét kell megadni. Lásd: [Azure-Blob adatkészlet tulajdonságai](../data-factory/data-factory-azure-blob-connector.md#dataset-properties) JSON használt tulajdonságok toodefine egy Azure Blob-adathalmazra vonatkozó további információért.  

```json

{
    "type": "datasets",
    "name": "[variables('blobOutputDatasetName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "[variables('storageLinkedServiceName')]",
        "typeProperties": {
            "folderPath": "adfgetstarted/partitioneddata",
            "format": {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability": {
            "frequency": "Month",
            "interval": 1,
            "style": "EndOfInterval"
        }
    }
}
```

hello folderPath hello elérési toohello mappa hello kimeneti adatokat tartalmazó határozza meg:

```json
"folderPath": "adfgetstarted/partitioneddata",
```

Hello [adatkészlet rendelkezésre állási](../data-factory/data-factory-create-datasets.md#dataset-availability) beállítás a következőképpen történik:

```json
"availability": {
    "frequency": "Month",
    "interval": 1,
    "style": "EndOfInterval"
},
```

Az Azure Data Factoryben, kimeneti adatkészlet rendelkezésre állási meghajtók hello folyamat. Ebben a példában hello szelet jön létre havonta hello (EndOfInterval) hónap utolsó napján meg. További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).

#### <a name="data-pipeline"></a>Adatfolyamat
Megadhatja egy folyamatot, amely átalakítja az adatok igény szerinti Azure HDInsight-fürtök a Hive parancsfájl futtatásával. Lásd: [adatcsatorna JSON](../data-factory/data-factory-create-pipelines.md#pipeline-json) a JSON használt elemek toodefine ebben a példában a folyamat leírását.

```json
{
    "type": "datapipelines",
    "name": "[parameters('dataFactoryName')]",
    "dependsOn": [
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobInputDatasetName'))]",
        "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/datasets/', variables('blobOutputDatasetName'))]"
    ],
    "apiVersion": "[variables('apiVersion')]",
    "properties": {
        "description": "Azure Data Factory pipeline with an Hadoop Hive activity",
        "activities": [
            {
                "type": "HDInsightHive",
                "typeProperties": {
                    "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                    "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                    "defines": {
                        "inputtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                        "partitionedtable": "[concat('wasb://adfgetstarted@', parameters('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
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
                "name": "RunSampleHiveActivity",
                "linkedServiceName": "HDInsightOnDemandLinkedService"
            }
        ],
        "start": "2016-01-01T00:00:00Z",
        "end": "2016-01-31T00:00:00Z",
        "isPaused": false
    }
}
```

hello adatcsatorna egy tevékenységet, HDInsightHive tevékenységet tartalmaz. Mivel a kezdő és záró dátumát 2016. január adatokat (a szeletek) feldolgozva csak egy hónap. Mindkét *start* és *end* múltbeli dátum, hello tevékenység rendelkezik, így a Data Factory hello hello hónap azonnal adatokat dolgozza fel. Ha hello célból egy jövőbeli dátumot, hello adat-előállító létrehoz egy másik szelet, ha hello idő. További információkért lásd: [Data Factory ütemezés és a végrehajtási](../data-factory/data-factory-scheduling-and-execution.md).

## <a name="clean-up-hello-tutorial"></a>Hello oktatóanyag tisztítása

### <a name="delete-hello-blob-containers-created-by-on-demand-hdinsight-cluster"></a>Igény szerinti HDInsight-fürt által létrehozott hello blobtárolók törlése
Az igény szerinti HDInsight kapcsolódó szolgáltatás használata esetén egy HDInsight-fürt létrehozása minden alkalommal, amikor a szelet kell feldolgozni, kivéve, ha egy meglévő élő fürthöz (élettartam); toobe és hello fürt akkor törlődnek, ha nem hello hajtja végre. Az egyes fürtökön Azure Data Factory egy blob-tároló hello hello fürthöz hello alapértelmezett stroage fiókként használt Azure blob Storage tárolóban hoz létre. Annak ellenére, hogy a HDInsight-fürtök törlése hello alapértelmezett blob tároló és a kapcsolódó hello tárfiók nem törlődik. Ez a működésmód szándékos. Ahogy egyre több szelet lesz feldolgozva, egyre több tároló jelenik meg az Azure Blob Storage-tárban. Ha nem kell őket hello feladatok hibaelhárítási, érdemes lehet a toodelete őket tooreduce hello tárolási költségeket. ezekhez a tárolókhoz hello nevei, hajtsa végre a minta: `adfyourdatafactoryname-linkedservicename-datetimestamp`.

Törölje a hello **adfjobs** és **adfyourdatafactoryname-linkedservicename-datetimestamp** mappák. hello adfjobs tároló feladatnaplóit az Azure Data Factory tartalmaz.

### <a name="delete-hello-resource-group"></a>Hello erőforráscsoport törlése
[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) van használt toodeploy, kezelheti és figyelheti a megoldás csoportként.  Erőforráscsoport törlése az összes hello összetevő hello csoporton belül.  

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **erőforráscsoportok** hello bal oldali ablaktáblán.
3. Kattintson az erőforráscsoport neve hello a PowerShell parancsfájl létrehozott. Hello szűrőt használhat, ha túl sok erőforrás-csoportok szerepel a listában. Hello erőforráscsoportot egy új panelen nyitja meg.
4. A hello **erőforrások** csempe, kell rendelkeznie, hello alapértelmezett tárfiókot és felsorolt, kivéve, ha hello erőforráscsoport megosztása más projektek hello adat-előállítóban.
5. Kattintson a **törlése** hello felül hello panelről. Ezzel törli a hello tárfiók és a storage-fiók hello hello adataihoz.
6. Írja be a hello erőforrás csoport neve tooconfirm törlése, és kattintson **törlése**.

Abban az esetben, ha nem kívánja toodelete hello tárfiók hello erőforráscsoport törlésekor, fontolja meg a következő architektúra hello alapértelmezett tárfiókból hello üzleti adatok elválasztva hello. Ebben az esetben rendelkezik egy erőforráscsoport hello tárfiók hello üzleti adatok, és egy másik erőforráscsoportban hello alapértelmezett tárfiók HDInsight csatolt szolgáltatás és hello adat-előállító hello. Hello második erőforráscsoport törlése nem érinti hello üzleti adatok tárfiók. toodo így:

* Adja hozzá a következő toohello legfelső szintű erőforráscsoport együtt hello Microsoft.DataFactory/datafactories erőforrás a Resource Manager sablon hello. Létrehoz egy tárfiókot:

    ```json
    {
        "name": "[parameters('defaultStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": {

        },
        "properties": {
            "accountType": "Standard_LRS"
        }
    },
    ```
* Új társított szolgáltatás pont toohello új tárfiók hozzáadása:

    ```json
    {
        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]" ],
        "type": "linkedservices",
        "name": "[variables('defaultStorageLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('defaultStorageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('defaultStorageAccountName')), variables('defaultApiVersion')).key1)]"
            }
        }
    },
    ```
* Egy további dependsOn és egy additionalLinkedServiceNames hello HDInsight ondemand kapcsolódó szolgáltatás konfigurálása:

    ```json
    {
        "dependsOn": [
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('defaultStorageLinkedServiceName'))]",
            "[concat('Microsoft.DataFactory/dataFactories/', parameters('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"

        ],
        "type": "linkedservices",
        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
        "apiVersion": "[variables('apiVersion')]",
        "properties": {
            "type": "HDInsightOnDemand",
            "typeProperties": {
                "version": "3.5",
                "clusterSize": 1,
                "timeToLive": "00:05:00",
                "osType": "Linux",
                "sshUserName": "myuser",                            
                "sshPassword": "MyPassword!",
                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                "additionalLinkedServiceNames": "[variables('defaultStorageLinkedServiceName')]"
            }
        }
    },            
    ```
## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toouse Azure Data Factory toocreate igény szerinti HDInsight-fürt tooprocess Hive-feladatokat. További tooread:

* [Hadoop oktatóanyag: hdinsight Linux-alapú Hadoop használatának megkezdése](hdinsight-hadoop-linux-tutorial-get-started.md)
* [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban](hdinsight-hadoop-provision-linux-clusters.md)
* [HDInsight-dokumentáció](https://azure.microsoft.com/documentation/services/hdinsight/)
* [Data factory dokumentáció](https://azure.microsoft.com/documentation/services/data-factory/)

## <a name="appendix"></a>Függelék:

### <a name="azure-cli-script"></a>Az Azure CLI-parancsfájlt
Használhatja az Azure CLI Azure PowerShell toodo hello oktatóanyag használata helyett. toouse Azure CLI-t, először telepítse az Azure parancssori felület hello utasításai szerint:

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

#### <a name="use-azure-cli-tooprepare-hello-storage-and-copy-hello-files"></a>Az Azure parancssori felület tooprepare hello tárhelyet használja, és hello fájlok másolása

```
azure login

azure config mode arm

azure group create --name "<Azure Resource Group Name>" --location "East US 2"

azure storage account create --resource-group "<Azure Resource Group Name>" --location "East US 2" --type "LRS" <Azure Storage Account Name>

azure storage account keys list --resource-group "<Azure Resource Group Name>" "<Azure Storage Account Name>"
azure storage container create "adfgetstarted" --account-name "<Azure Storage AccountName>" --account-key "<Azure Storage Account Key>"

azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/inputdata/input.log" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
azure storage blob copy start "https://hditutorialdata.blob.core.windows.net/adfhiveactivity/script/partitionweblogs.hql" --dest-account-name "<Azure Storage Account Name>" --dest-account-key "<Azure Storage Account Key>" --dest-container "adfgetstarted"
```

hello tároló neve *adfgetstarted*. Legyen ez. Ellenkező esetben tooupdate hello Resource Manager-sablon van szüksége. Ha a parancssori felület parancsfájllal segítségre van szüksége, tekintse meg [az Azure Storage Azure CLI használata hello](../storage/common/storage-azure-cli.md).
