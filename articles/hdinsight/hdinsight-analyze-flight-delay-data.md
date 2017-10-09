---
title: "aaaAnalyze repülési késleltetés adatok hadooppal a Hdinsightban - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy Windows PowerShell parancsfájl toocreate HDInsight-fürtöt, a Hive feladat futtatása Sqoop feladatot futtatni, és hello fürt törlése."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00e26aa9-82fb-4dbe-b87d-ffe8e39a5412
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 6ebaee65d9b270e5dc2141dd1265011d372f497d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-flight-delay-data-by-using-hive-in-hdinsight"></a>Repülési késleltetés adatok elemzése a Hive HDInsight használatával
Hive lehetővé teszi egy SQL-szerű nevű programozási nyelv használatával a feladatok Hadoop MapReduce futó  *[HiveQL][hadoop-hiveql]*, amelyek alkalmazhatók felé összefoglalójához, kérdez le, és nagy mennyiségű adatot elemzése.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések szükséges egy Windows-alapú HDInsight-fürtöt. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). A Linux-alapú fürt lépéseiért lásd: [repülési késleltetés adatok elemzése a Hive HDInsight (Linux) használatával](hdinsight-analyze-flight-delay-data-linux.md).

Adatok tárolási és számítási hello elkülönítése egyik fő előnye hello Azure HDInsight. HDInsight Azure Blob storage-tárolására használ. Egy tipikus munkába beletartozik a három részből áll:

1. **Adatok tárolása az Azure Blob Storage tárolóban.**  Például időjárás adatok, érzékelőadatait, webes naplókat, és ebben az esetben repülési késleltetés adatok menti azokat Azure Blob Storage tárolóban.
2. **Feladatok futtatása.** Ha tooprocess hello időadatok, futtatja egy Windows PowerShell-parancsfájlt (vagy egy ügyfélalkalmazás) toocreate HDInsight-fürtöt, feladatok futtatása, és hello fürt törlése. Mentse a kimeneti adatok tooAzure Blob-tároló hello feladatok. hello kimeneti adatok hello fürtök törlése után is megmarad. Ezzel a módszerrel kell fizetnie csak néhány Ön által igénybe vett.
3. **Hello kimeneti beolvasása az Azure Blob storage**, vagy az oktatóanyagban hello adatok tooan Azure SQL adatbázis-exportálási.

hello következő diagram azt ábrázolja, hello forgatókönyv és ebben az oktatóanyagban hello szerkezete:

![HDI. FlightDelays.flow][img-hdi-flightdelays-flow]

Vegye figyelembe, hogy hello számok hello diagramon megfelelnek-e toohello szakaszok címét. **M** hello fő folyamat jelenti. **A** hello tartalom hello függelékben jelenti.

hello fő része hello az oktatóanyag bemutatja, hogyan toouse egy Windows PowerShell parancsfájl tooperform hello a következő feladatokat:

* HDInsight-fürtök létrehozása.
* Hello fürt toocalculate átlagos késleltetése repülőtereken egy Hive-feladat fut. egy Azure Blob storage-fiók hello repülési késleltetés adatokat tárolja.
* Sqoop feladat tooexport hello Hive feladat kimeneti tooan Azure SQL-adatbázis futtatásához.
* Hello HDInsight-fürt törlése.

Hello függelékben találhat repülési késleltetés adatok feltöltése, a Hive lekérdezési karakterlánc létrehozása/feltöltése és hello Azure SQL-adatbázis előkészítése hello Sqoop feladat hello útmutatót.

> [!NOTE]
> hello jelen dokumentumban leírt lépések olyan adott tooWindows-alapú HDInsight-fürtök esetében. A Linux-alapú fürt lépéseiért lásd: [adatelemzés repülési késleltetés segítségével Hive HDInsight (Linux)](hdinsight-analyze-flight-delay-data-linux.md)

### <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez a következő elemek hello kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Munkaállomás Azure PowerShell-lel**.

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.

**Ebben az oktatóanyagban használt fájlok**

Ez az oktatóanyag használja hello időben teljesítményének légitársaság felé továbbított adatok [kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda vagy RITA][rita-website].
Hello adatok másolatát feltöltött tooan Azure Blob storage tárolók hello nyilvános Blob hozzáférési engedélyekkel.
A PowerShell parancsfájl részét hello adatok hello nyilvános blob tárolóban toohello alapértelmezett blob tároló a fürt másolja át. hello HiveQL-parancsfájlt is van másolt toohello Blob tárolóhoz.
Ha azt szeretné, hogy toolearn hogyan tooget/feltöltés hello adatok tooyour saját tárfiókot, és hogyan toocreate/feltöltés hello HiveQL parancsfájl-fájlt, tekintse meg a [függelék](#appendix-a) és [B függelék](#appendix-b).

hello következő táblázat sorolja fel ebben az oktatóanyagban használt hello fájlok:

<table border="1">
<tr><th>Fájlok</th><th>Leírás</th></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/flightdelays.hql</td><td>hello Hive feladat használják hello HiveQL-parancsfájlt. Ez a parancsfájl lett feltöltött tooan hello nyilvános hozzáférés az Azure Blob storage-fiók. <a href="#appendix-b">B függelék</a> előkészítése, majd ismét feltölteni a fájlt tooyour saját Azure Blob storage-fiók utasításokat tartalmaz.</td></tr>
<tr><td>wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data</td><td>A bemeneti adatok hello Hive feladat. hello már feltöltött tooan hello nyilvános hozzáférés az Azure Blob storage-fiók. <a href="#appendix-a">A függelék</a> utasításokat rendelkezzen hello adatokat, majd ismét feltölteni a hello adatok tooyour saját Azure Blob storage-fiók.</td></tr>
<tr><td>\tutorials\flightdelays\output</td><td>hello kimeneti elérési út hello Hive feladat. hello alapértelmezett tároló hello kimeneti adatok tárolására szolgál.</td></tr>
<tr><td>\tutorials\flightdelays\jobstatus</td><td>hello Hive feladat állapota mappájába hello alapértelmezett tárolót.</td></tr>
</table>

## <a name="create-cluster-and-run-hivesqoop-jobs"></a>Fürt létrehozása és Hive/Sqoop feladatok futtatása
Hadoop-MapReduce kötegelt feldolgozásával. hello legtöbb költséghatékony toorun egy Hive-feladat toocreate egy fürt hello feladat, és hello feladat törlése hello feladat befejezése után. a következő parancsfájl magában foglalja az hello teljes folyamat hello.
A HDInsight-fürtök létrehozása és Hive-feladatok futtatása további információkért lásd: [Hadoop létrehozása a HDInsight-fürtök] [ hdinsight-provision] és [használata a HDInsight Hive] [hdinsight-use-hive].

**toorun hello Hive-lekérdezések Azure PowerShell**

1. Hozzon létre egy Azure SQL database és hello tábla hello Sqoop feladat kimeneti hello utasításait [C függelék](#appendix-c).
2. Nyissa meg a Windows PowerShell ISE, és a következő parancsfájl hello:

    ```powershell
    $subscriptionID = "<Azure Subscription ID>"
    $nameToken = "<Enter an Alias>"

    ###########################################
    # You must configure hello follwing variables
    # for an existing Azure SQL Database
    ###########################################
    $existingSqlDatabaseServerName = "<Azure SQL Database Server>"
    $existingSqlDatabaseLogin = "<Azure SQL Database Server Login>"
    $existingSqlDatabasePassword = "<Azure SQL Database Server login password>"
    $existingSqlDatabaseName = "<Azure SQL Database name>"

    $localFolder = "E:\Tutorials\Downloads\" # A temp location for copying files.
    $azcopyPath = "C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy" # depends on hello version, hello folder can be different

    ###########################################
    # (Optional) configure hello following variables
    ###########################################

    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2"

    $HDInsightClusterName = $namePrefix + "hdi"
    $httpUserName = "admin"
    $httpPassword = "<Enter hello Password>"

    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $HDInsightClusterName # use hello cluster name

    $existingSqlDatabaseTableName = "AvgDelays"
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$existingSqlDatabaseServerName.database.windows.net;user=$existingSqlDatabaseLogin@$existingSqlDatabaseServerName;password=$existingSqlDatabaseLogin;database=$existingSqlDatabaseName"

    $hqlScriptFile = "/tutorials/flightdelays/flightdelays.hql"

    $jobStatusFolder = "/tutorials/flightdelays/jobstatus"

    ###########################################
    # Login
    ###########################################
    try{
        $acct = Get-AzureRmSubscription
    }
    catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionID $subscriptionID

    ###########################################
    # Create a new HDInsight cluster
    ###########################################

    # Create ARM group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create hello default storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $existingDefaultBlobContainerName

    ###########################################
    # Prepare hello HiveQL script and source data
    ###########################################

    # Create hello temp location
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download hello sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous
    $blobs = Get-AzureStorageBlob -Container "flightdelay" -Context $context
    #$blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload data toodefault container

    $azcopycmd = "cmd.exe /C '$azcopyPath\azcopy.exe' /S /Source:'$localFolder' /Dest:'https://$defaultStorageAccountName.blob.core.windows.net/$defaultBlobContainerName/tutorials/flightdelays' /DestKey:$defaultStorageAccountKey"

    Invoke-Expression -Command:$azcopycmd

    ###########################################
    # Submit hello Hive job
    ###########################################
    Use-AzureRmHDInsightCluster -ClusterName $HDInsightClusterName -HttpCredential $httpCredential
    $response = Invoke-AzureRmHDInsightHiveJob `
                    -Files $hqlScriptFile `
                    -DefaultContainer $defaultBlobContainerName `
                    -DefaultStorageAccountName $defaultStorageAccountName `
                    -DefaultStorageAccountKey $defaultStorageAccountKey `
                    -StatusFolder $jobStatusFolder

    write-Host $response

    ###########################################
    # Submit hello Sqoop job
    ###########################################
    $exportDir = "wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net/tutorials/flightdelays/output"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
                    -Command "export --connect $sqlDatabaseConnectionString --table $sqlDatabaseTableName --export-dir $exportDir --fields-terminated-by \001 "
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ResourceGroupName $resourceGroupName `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -HttpCredential $httpCredential `
        -WaitTimeoutInSeconds 3600 `
        -Job $sqoopJob.JobId

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -DefaultContainer $existingDefaultBlobContainerName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    ###########################################
    # Delete hello cluster
    ###########################################
    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName
    ```
3. Csatlakozás tooyour SQL-adatbázis, és átlagos repülési késések hello AvgDelays tábla városonként lásd:

    ![HDI. FlightDelays.AvgDelays.Dataset][image-hdi-flightdelays-avgdelays-dataset]

- - -

## <a id="appendix-a"></a>A függelék – feltöltés repülési késleltetés adatok tooAzure Blob-tároló
Hello adatfájl és -hello HiveQL parancsfájl fájlok feltöltése (lásd: [B függelék](#appendix-b)) is szükség van néhány. hello lényege toostore hello az adatfájlok és hello HiveQL fájl HDInsight fürtök létrehozásával és hello Hive feladat futtatása előtt. Erre két lehetősége van:

* **Használja az azonos hello hello alapértelmezett fájlrendszer hello HDInsight-fürt által használandó Azure Storage-fiók.** Mivel a HDInsight-fürt hello hello tárfiók_elérési_kulcsa lesz, a további módosításokat toomake nem szükséges.
* **Hello HDInsight fürt alapértelmezett fájlrendszer egy másik Azure Storage-fiókot használni.** Ha ez helyzet hello, módosítania kell a Windows PowerShell parancsfájl található hello hello létrehozása részét [hozzon létre HDInsight-fürtöt és futtatási Hive/Sqoop feladatok](#runjob) toolink hello tárfiók egy további tárfiókkal. Útmutatásért lásd: [Hadoop létrehozása a HDInsight-fürtök][hdinsight-provision]. hello HDInsight-fürt majd tudja hello tárfiók hello a hozzáférési kulcsot.

> [!NOTE]
> hello Blob storage a hello adatfájl elérési út rögzített a kódolt hello HiveQL-parancsfájlt. Ennek megfelelően kell frissíteni.

**toodownload hello felé továbbított adatok**

1. Keresse meg a túl[kutatási és innovatív technológia felügyeleti, szállítására statisztika iroda][rita-website].
2. Hello lapon jelölje be a következő értékek hello:

    <table border="1">
    <tr><th>Név</th><th>Érték</th></tr>
    <tr><td>Szűrő év</td><td>2013 </td></tr>
    <tr><td>Időszak szűrése</td><td>Január</td></tr>
    <tr><td>Mezők</td><td>*Év*, *FlightDate*, *UniqueCarrier*, *szolgáltatónként*, *FlightNum*, *OriginAirportID* , *Származási*, *OriginCityName*, *OriginState*, *DestAirportID*, *cél* , *DestCityName*, *DestState*, *DepDelayMinutes*, *ArrDelay*,  *ArrDelayMinutes*, *CarrierDelay*, *WeatherDelay*, *NASDelay*, *SecurityDelay*,  *LateAircraftDelay* (a többi mező törlése)</td></tr>
    </table>
3.Kattintson a **letöltése**.
4. Bontsa ki a hello fájl toohello **C:\Tutorials\FlightDelay\2013Data** mappa. Minden fájlt egy CSV-fájl, és körülbelül 60 GB-nál.
5. Nevezze át a toohello fájlnév hello hello hónap, amely adatokat tartalmaz. Hello január adatokat tartalmazó hello fájlt volna neve például *January.csv*.
6. Ismételje meg a 2. és 5 toodownload fájl az egyes hello 2013 12 hónapig. Szüksége lesz legalább egy fájl toorun hello oktatóanyag.

**tooupload hello repülési késleltetés adatok tooAzure Blob-tároló**

1. Készítse elő a hello paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Megjegyzések</th></tr>
    <tr><td>$storageAccountName</td><td>hello Azure Storage-fiókra, amelyhez tooupload hello adatokat.</td></tr>
    <tr><td>$blobContainerName</td><td>hello Blob tároló, ahol szeretné tooupload hello adatokat.</td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.
3. Illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:

    ```powershell
    [CmdletBinding()]
    Param(

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #Region - Variables
    $localFolder = "C:\Tutorials\FlightDelay\2013Data"  # hello source folder
    $destFolder = "tutorials/flightdelay/2013data"     #hello blob name prefix for hello files toobe uploaded
    #EndRegion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #Region - Copy hello file from local workstation tooAzure Blob storage
    if (test-path -Path $localFolder)
    {
        foreach ($item in Get-ChildItem -Path $localFolder){
            $fileName = "$localFolder\$item"
            $blobName = "$destFolder/$item"

            Write-Host "Copying $fileName too$blobName" -ForegroundColor Green

            Set-AzureStorageBlobContent -File $fileName -Container $blobContainerName -Blob $blobName -Context $storageContext
        }
    }
    else
    {
        Write-Host "hello source folder on hello workstation doesn't exist" -ForegroundColor Red
    }

    # List hello uploaded files on HDInsight
    Get-AzureStorageBlob -Container $blobContainerName  -Context $storageContext -Prefix $destFolder
    #EndRegion
    ```
4. Nyomja le az **F5** toorun hello parancsfájl.

Ha úgy dönt, toouse hello fájlok feltöltése más módszert, ellenőrizze, hogy hello fájl elérési útja oktatóanyagok/flightdelay/adatokat. hello hello fájlokhoz való hozzáférést szintaxisa a következő:

    wasb://<ContainerName>@<StorageAccountName>.blob.core.windows.net/tutorials/flightdelay/data

hello elérési oktatóanyagok/flightdelay/adata hello hello fájlok feltöltött létrehozott virtuális mappát. Győződjön meg arról, hogy vannak-e egy minden hónapban a 12 fájlt.

> [!NOTE]
> Hello Hive lekérdezés tooread hello új helyről kell frissíteni.
>
> Hello tároló hozzáférési engedélyt toobe nyilvános konfigurálásához, vagy kötési hello tárolási fiók toohello HDInsight-fürthöz. Ellenkező esetben hello Hive lekérdezés-karakterlánc nem lesz képes tooaccess hello adatfájlokat.

- - -

## <a id="appendix-b"></a>B függelék – hozzon létre, és töltse fel a HiveQL-parancsfájlt
Az Azure PowerShell használatával is futtathatja több HiveQL utasítást egy időben, vagy csomag hello HiveQL utasítás egy parancsfájl-fájlba. Ez a szakasz bemutatja, hogyan toocreate HiveQL-parancsfájlt és a feltöltés hello parancsfájl tooAzure a Blob storage Azure PowerShell használatával. Hive hello HiveQL parancsfájlok toobe Azure Blob storage-ban tárolt igényel.

hello HiveQL-parancsfájlt hello következőket hajtja végre:

1. **Hello delays_raw table DROP**, abban az esetben hello tábla már létezik.
2. **Hello delays_raw külső Hive tábla létrehozásához** toohello Blob tároló helye mutató hello repülési késleltetés fájlokkal. Ez a lekérdezés határozza meg, hogy mezők határolja ",", és hogy sorok "\n" vannak-e állítva. Ez problémát jelent, amikor a mezőértékek tartalmazhat vesszőket, mert struktúra nem különbözteti meg, amely a mezőhatároló vesszővel és a mezőérték része egy (vagyis mezőjének értékét származási hello nagybetűkkel\_VÁROS\_nevét, és a cél\_ VÁROS\_neve). tooaddress a, hello lekérdezés adatokat hoz létre TEMP oszlopok toohold helytelenül osztott oszlopok.
3. **Hello késések table DROP**, abban az esetben hello tábla már létezik.
4. **Hello késések tábla létrehozása**. További feldolgozás előtt is hasznos tooclean hello adatokat. Ez a lekérdezés táblát hoz létre új *késések*, hello delays_raw táblából. Vegye figyelembe, hogy a rendszer nem másolja hello átmeneti oszlopokban (ahogy korábban említettük), és adott hello **substring** függvény használt tooremove idézőjelek hello adatokból.
5. **Számítási hello átlagos időjárási késleltetés és a csoportok hello eredmények város név szerint.** Azt is kimeneteként hello eredmények tooBlob tároló. Vegye figyelembe a hello lekérdezés eltávolítja az aposztrófot hello adatokból, és kihagyja a sort, ahol hello értékének **weather_delay** null értékű. Erre akkor szükség, mert a Sqoop, az oktatóanyag későbbi részében használt nem kezeli az ezeket az értékeket szabályosan alapértelmezés szerint.

Hello HiveQL parancsok teljes listáját lásd: [Hive adatdefiníciós nyelv][hadoop-hiveql]. Minden HiveQL parancs pontosvesszővel kell állítanunk.

**toocreate HiveQL-parancsfájlt**

1. Készítse elő a hello paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Megjegyzések</th></tr>
    <tr><td>$storageAccountName</td><td>hello tooupload hello HiveQL-parancsfájlt a kívánt Azure Storage-fiók.</td></tr>
    <tr><td>$blobContainerName</td><td>Ha azt szeretné, hogy tooupload hello HiveQL-parancsfájlt a Blob-tároló hello.</td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.
3. Másolja és illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Blob storage variables
        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure storage account name for creating a new HDInsight cluster. If hello account doesn't exist, hello script will create one.")]
        [String]$storageAccountName,

        [Parameter(Mandatory=$True,
                    HelpMessage="Enter hello Azure blob container name for creating a new HDInsight cluster. If not specified, hello HDInsight cluster name will be used.")]
        [String]$blobContainerName
    )

    #region - Define variables
    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    # hello HiveQL script file is exported as this file before it's uploaded tooBlob storage
    $hqlLocalFileName = "e:\tutorials\flightdelay\flightdelays.hql"

    # hello HiveQL script file will be uploaded tooBlob storage as this blob name
    $hqlBlobName = "tutorials/flightdelay/flightdelays.hql"

    # These two constants are used by hello HiveQL script file
    #$srcDataFolder = "tutorials/flightdelay/data"
    $dstDataFolder = "/tutorials/flightdelay/output"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #Region - Validate user input
    Write-Host "`nValidating hello Azure Storage account and hello Blob container..." -ForegroundColor Green
    # Validate hello Storage account
    if (-not (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}))
    {
        Write-Host "hello storage account, $storageAccountName, doesn't exist." -ForegroundColor Red
        exit
    }
    else{
        $resourceGroupName = (Get-AzureRmStorageAccount|Where-Object{$_.StorageAccountName -eq $storageAccountName}).ResourceGroupName
    }

    # Validate hello container
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    if (-not (Get-AzureStorageContainer -Context $storageContext |Where-Object{$_.Name -eq $blobContainerName}))
    {
        Write-Host "hello Blob container, $blobContainerName, doesn't exist" -ForegroundColor Red
        Exit
    }
    #EngRegion

    #region - Validate hello file and file path

    # Check if a file with hello same file name already exists on hello workstation
    Write-Host "`nvalidating hello folder structure on hello workstation for saving hello HQL script file ..."  -ForegroundColor Green
    if (test-path $hqlLocalFileName){

        $isDelete = Read-Host 'hello file, ' $hqlLocalFileName ', exists.  Do you want toooverwirte it? (Y/N)'

        if ($isDelete.ToLower() -ne "y")
        {
            Exit
        }
    }

    # Create hello folder if it doesn't exist
    $folder = split-path $hqlLocalFileName
    if (-not (test-path $folder))
    {
        Write-Host "`nCreating folder, $folder ..." -ForegroundColor Green

        new-item $folder -ItemType directory
    }
    #end region

    #region - Write hello Hive script into a local file
    Write-Host "`nWriting hello Hive script into a file on your workstation ..." `
                -ForegroundColor Green

    $hqlDropDelaysRaw = "DROP TABLE delays_raw;"

    $hqlCreateDelaysRaw = "CREATE EXTERNAL TABLE delays_raw (" +
            "YEAR string, " +
            "FL_DATE string, " +
            "UNIQUE_CARRIER string, " +
            "CARRIER string, " +
            "FL_NUM string, " +
            "ORIGIN_AIRPORT_ID string, " +
            "ORIGIN string, " +
            "ORIGIN_CITY_NAME string, " +
            "ORIGIN_CITY_NAME_TEMP string, " +
            "ORIGIN_STATE_ABR string, " +
            "DEST_AIRPORT_ID string, " +
            "DEST string, " +
            "DEST_CITY_NAME string, " +
            "DEST_CITY_NAME_TEMP string, " +
            "DEST_STATE_ABR string, " +
            "DEP_DELAY_NEW float, " +
            "ARR_DELAY_NEW float, " +
            "CARRIER_DELAY float, " +
            "WEATHER_DELAY float, " +
            "NAS_DELAY float, " +
            "SECURITY_DELAY float, " +
            "LATE_AIRCRAFT_DELAY float) " +
        "ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' " +
        "LINES TERMINATED BY '\n' " +
        "STORED AS TEXTFILE " +
        "LOCATION 'wasb://flightdelay@hditutorialdata.blob.core.windows.net/2013Data';"

    $hqlDropDelays = "DROP TABLE delays;"

    $hqlCreateDelays = "CREATE TABLE delays AS " +
        "SELECT YEAR AS year, " +
            "FL_DATE AS flight_date, " +
            "substring(UNIQUE_CARRIER, 2, length(UNIQUE_CARRIER) -1) AS unique_carrier, " +
            "substring(CARRIER, 2, length(CARRIER) -1) AS carrier, " +
            "substring(FL_NUM, 2, length(FL_NUM) -1) AS flight_num, " +
            "ORIGIN_AIRPORT_ID AS origin_airport_id, " +
            "substring(ORIGIN, 2, length(ORIGIN) -1) AS origin_airport_code, " +
            "substring(ORIGIN_CITY_NAME, 2) AS origin_city_name, " +
            "substring(ORIGIN_STATE_ABR, 2, length(ORIGIN_STATE_ABR) -1)  AS origin_state_abr, " +
            "DEST_AIRPORT_ID AS dest_airport_id, " +
            "substring(DEST, 2, length(DEST) -1) AS dest_airport_code, " +
            "substring(DEST_CITY_NAME,2) AS dest_city_name, " +
            "substring(DEST_STATE_ABR, 2, length(DEST_STATE_ABR) -1) AS dest_state_abr, " +
            "DEP_DELAY_NEW AS dep_delay_new, " +
            "ARR_DELAY_NEW AS arr_delay_new, " +
            "CARRIER_DELAY AS carrier_delay, " +
            "WEATHER_DELAY AS weather_delay, " +
            "NAS_DELAY AS nas_delay, " +
            "SECURITY_DELAY AS security_delay, " +
            "LATE_AIRCRAFT_DELAY AS late_aircraft_delay " +
        "FROM delays_raw;"

    $hqlInsertLocal = "INSERT OVERWRITE DIRECTORY '$dstDataFolder' " +
        "SELECT regexp_replace(origin_city_name, '''', ''), " +
            "avg(weather_delay) " +
        "FROM delays " +
        "WHERE weather_delay IS NOT NULL " +
        "GROUP BY origin_city_name;"

    $hqlScript = $hqlDropDelaysRaw + $hqlCreateDelaysRaw + $hqlDropDelays + $hqlCreateDelays + $hqlInsertLocal

    $hqlScript | Out-File $hqlLocalFileName -Encoding ascii -Force
    #endregion

    #region - Upload hello Hive script toohello default Blob container
    Write-Host "`nUploading hello Hive script toohello default Blob container ..." -ForegroundColor Green

    # Create a storage context object
    $storageAccountKey = (Get-AzureRmStorageAccountKey -StorageAccountName $storageAccountName -ResourceGroupName $resourceGroupName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

    # Upload hello file from local workstation tooBlob storage
    Set-AzureStorageBlobContent -File $hqlLocalFileName -Container $blobContainerName -Blob $hqlBlobName -Context $destContext
    #endregion

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

    Az alábbiakban hello parancsfájlban használt hello változók:

   * **$hqlLocalFileName** -hello parancsfájl menti hello HiveQL-parancsfájlt helyileg feltöltés előtt tooBlob tároló. Ez az hello fájl nevét. hello alapértelmezett értéke <u>C:\tutorials\flightdelay\flightdelays.hql</u>.
   * **$hqlBlobName** -hello HiveQL parancsfájl fájl blob neve szerepel a hello Azure Blob Storage tárolóban. hello alapértelmezett értéke tutorials/flightdelay/flightdelays.hql. Hello fájlba lesz írva közvetlenül tooAzure Blob-tároló, mert nincs olyan "/" hello blob neve hello elején. Ha azt szeretné, hogy a Blob storage tooaccess hello fájlt, akkor tooadd, a "/" hello fájlnév hello elején.
   * **$srcDataFolder** és **$dstDataFolder** -= "oktatóanyagok/flightdelay/adatok" = "oktatóanyagok/flightdelay/kimeneti"

- - -
## <a id="appendix-c"></a>C függelék – hello Sqoop feladatkiemenetét Azure SQL-adatbázis előkészítése
**tooprepare hello SQL-adatbázis (egyesítés ezzel a Sqoop parancsfájl hello)**

1. Készítse elő a hello paraméterek:

    <table border="1">
    <tr><th>Változó neve</th><th>Megjegyzések</th></tr>
    <tr><td>$sqlDatabaseServerName</td><td>hello hello Azure SQL adatbázis-kiszolgáló nevét. Adja meg, mit toocreate egy új kiszolgálóra.</td></tr>
    <tr><td>$sqlDatabaseUsername</td><td>hello bejelentkezési név hello Azure SQL adatbázis-kiszolgáló. Ha $sqlDatabaseServerName egy meglévő kiszolgálóról, hello bejelentkezési és a bejelentkezés jelszó is használt tooauthenticate hello kiszolgálóval. Ellenkező esetben azok használt toocreate egy új kiszolgálóra.</td></tr>
    <tr><td>$sqlDatabasePassword</td><td>a bejelentkezés jelszó hello hello Azure SQL adatbázis-kiszolgáló.</td></tr>
    <tr><td>$sqlDatabaseLocation</td><td>Ezt az értéket csak akkor, ha hoz létre egy új Azure-adatbázis-kiszolgálót használja.</td></tr>
    <tr><td>$sqlDatabaseName</td><td>SQL-adatbázis hello toocreate hello AvgDelays tábla hello Sqoop feladathoz használt. Hagyja üresen HDISqoop nevű adatbázist fog létrehozni. hello táblaneve hello Sqoop feladatkiemenetét AvgDelays. </td></tr>
    </table>
2. Nyissa meg az Azure PowerShell ISE.
3. Másolja és illessze be a következő parancsfájl hello parancsfájl ablaktáblára hello:

    ```powershell
    [CmdletBinding()]
    Param(

        # Azure Resource group variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure resource group name. It will be created if it doesn't exist.")]
        [String]$resourceGroupName,

        # SQL database server variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database Server Name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseServer,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user.")]
        [String]$sqlDatabaseLogin,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello Azure SQL Database admin user password.")]
        [String]$sqlDatabasePassword,

        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello region toocreate hello Database in.")]
        [String]$sqlDatabaseLocation,   #For example, West US.

        # SQL database variables
        [Parameter(Mandatory=$True,
                HelpMessage="Enter hello database name. It will be created if it doesn't exist.")]
        [String]$sqlDatabaseName # specify hello database name if you have one created. Otherwise use "" toohave hello script create one for you.
    )

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Constants and variables

    # IP address REST service used for retrieving external IP address and creating firewall rules
    [String]$ipAddressRestService = "http://bot.whatismyipaddress.com"
    [String]$fireWallRuleName = "FlightDelay"

    # SQL database variables
    [String]$sqlDatabaseMaxSizeGB = 10

    #SQL query string for creating AvgDelays table
    [String]$sqlDatabaseTableName = "AvgDelays"
    [String]$sqlCreateAvgDelaysTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [origin_city_name] [nvarchar](50) NOT NULL,
                [weather_delay] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
                [origin_city_name] ASC
            )
            )"
    #endregion

    #Region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #EndRegion

    #region - Create and validate Azure resouce group
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $sqlDatabaseLocation
    }

    #EndRegion

    #region - Create and validate Azure SQL database server
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServer -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServer = (New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -SqlAdministratorCredentials $credential -Location $sqlDatabaseLocation).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServer." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-workstation" -StartIpAddress $workstationIPAddress -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -FirewallRuleName "$fireWallRuleName-Azureservices" -StartIpAddress "0.0.0.0" -EndIpAddress "0.0.0.0"
    }

    #endregion

    #region - Create and validate Azure SQL database

    try {
        Get-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $sqlDatabaseServer -DatabaseName $sqlDatabaseName -Edition "Standard" -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region -  Execute an SQL command toocreate hello AvgDelays table

    Write-Host "`nCreating SQL Database table ..."  -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = $sqlCreateAvgDelaysTable
    $cmd.executenonquery()

    $conn.close()

    Write-host "`nEnd of hello PowerShell script" -ForegroundColor Green
    ```

   > [!NOTE]
   > hello parancsfájl használ egy representational állapot transfer (REST) szolgáltatás, http://bot.whatismyipaddress.com, tooretrieve a külső IP-címet. hello IP-címet az SQL database-kiszolgálóhoz egy tűzfalszabályt létrehozására szolgál.

    Az alábbiakban néhány hello parancsfájlban használt változók:

   * **$ipAddressRestService** -hello alapértelmezett értéke http://bot.whatismyipaddress.com. Egy nyilvános IP-címet a külső IP-cím beolvasása a REST-szolgáltatást is. Más szolgáltatások is használhatja, ha azt szeretné. hello hello szolgáltatás segítségével külső IP-cím lesz használt toocreate az Azure SQL adatbázis-kiszolgáló, egy tűzfalszabályt, így (a Windows PowerShell-parancsfájl használatával) a munkaállomáson hello adatbázis végezheti el.
   * **$fireWallRuleName** -hello Azure SQL adatbázis-kiszolgáló hello hello tűzfalszabály neve. hello alapértelmezés szerint ez <u>FlightDelay</u>. Ha azt szeretné, átnevezheti.
   * **$sqlDatabaseMaxSizeGB** -Ez az érték csak egy új Azure SQL adatbázis-kiszolgáló létrehozásakor használható. hello alapértelmezett értéke 10 GB-os. 10 GB-os is elegendő ehhez az oktatóanyaghoz.
   * **$sqlDatabaseName** -Ez az érték csak egy új Azure SQL-adatbázis létrehozásakor használható. hello alapértelmezett értéke HDISqoop. Ha az Átnevezés hello Sqoop Windows PowerShell-parancsfájl ennek megfelelően kell frissíteni.
4. Nyomja le az **F5** toorun hello parancsfájl.
5. Hello parancsfájl kimenetében ellenőrzése. Ellenőrizze, hogy hello parancsprogram sikeresen lefutott.

## <a id="nextsteps"></a> Következő lépések
Most már megismerte hogyan tooupload egy fájl tooAzure Blob-tároló, hogyan toopopulate egy Hive tábla hello adatok az Azure Blob storage használatával hogyan toorun Hive lekérdezések, és hogyan toouse HDFS tooan Azure SQL adatbázis Sqoop tooexport adatait. toolearn több, tekintse meg a következő cikkek hello:

* [Első lépései a hdinsight eszközzel][hdinsight-get-started]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [Oozie használata a hdinsight eszközzel][hdinsight-use-oozie]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[rita-website]: http://www.transtats.bts.gov/DL_SelectFields.asp?Table_ID=236&DB_Short_Name=On-Time
[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[hadoop-hiveql]: https://cwiki.apache.org/confluence/display/Hive/LanguageManual+DDL
[hadoop-shell-commands]: http://hadoop.apache.org/docs/r0.18.3/hdfs_shell.html

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx

[image-hdi-flightdelays-avgdelays-dataset]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.AvgDelays.DataSet.png
[img-hdi-flightdelays-run-hive-job-output]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.RunHiveJob.Output.png
[img-hdi-flightdelays-flow]: ./media/hdinsight-analyze-flight-delay-data/HDI.FlightDelays.Flow.png
