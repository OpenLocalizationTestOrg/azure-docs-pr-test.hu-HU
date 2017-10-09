---
title: hdinsight Hadoop Oozie aaaUse |} Microsoft Docs
description: "Hadoop Oozie használata a Hdinsightban, big data-szolgáltatása. Megtudhatja, hogyan toodefine egy Oozie munkafolyamatot, és az Oozie feladat elküldéséhez."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 870098f0-f416-4491-9719-78994bf4a369
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 12d0cf1a01838ab0f4e699c384ce2fb18f85cbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-in-hdinsight"></a>Hadoop toodefine Oozie használjon, és a munkafolyamat futtatását a Hdinsightban
[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Ismerje meg, hogyan toouse Apache Oozie toodefine munkafolyamat és a Futtatás hello munkafolyamat a hdinsight platformon. toolearn hello Oozie coordinator, kapcsolatban lásd: [időalapú Hadoop Oozie-koordinátor használata a HDInsight][hdinsight-oozie-coordinator-time]. Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].

Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli. Hadoop-veremmel hello integrálva van, és támogatja a Hadoop-feladatokat Apache MapReduce, Apache Pig, Apache Hive és Apache Sqoop. Azt is, amelyek adott tooa rendszer, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok.

Ebben az oktatóanyagban hello utasításokat követve megvalósítása hello munkafolyamat két műveletet tartalmaz:

![Munkafolyamat diagramja][img-workflow-diagram]

1. A Hive művelet a futás a HiveQL parancsfájl toocount hello napló szintű típusonkénti előfordulások log4j fájlban. Minden egyes log4j fájl megjelenítő hello típusától és súlyosságától hello, például [NAPLÓZÁSI szint] mező tartalmazó sor mezők áll:
   
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
   
    hello Hive parancsfájl kimenetében hasonlít:
   
        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4
   
    További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].
2. A Sqoop művelet hello HiveQL tooa eredménytábla Azure SQL adatbázis exportálja. További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].

> [!NOTE]
> Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?] [hdinsight-versions].
> 
> 

### <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag megkezdése előtt rendelkeznie kell hello elem a következő:

* **Munkaállomás Azure PowerShell-lel**. 
  

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
  

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a>Adja meg az Oozie munkafolyamat és a hello kapcsolódó HiveQL-parancsfájlt
Oozie munkafolyamatok definíciók hPDL (egy XML folyamat Definition Language) nyelven íródtak. hello alapértelmezett munkafolyamat Fájlnév *workflow.xml*. hello az alábbiakban az hello munkafolyamat fájl ebben az oktatóanyagban használja.

    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>

        <action name="RunHiveScript">
            <hive xmlns="uri:oozie:hive-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.job.queue.name</name>
                        <value>${queueName}</value>
                    </property>
                </configuration>
                <script>${hiveScript}</script>
                <param>hiveTableName=${hiveTableName}</param>
                <param>hiveDataFolder=${hiveDataFolder}</param>
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
            </hive>
            <ok to="RunSqoopExport"/>
            <error to="fail"/>
        </action>

        <action name="RunSqoopExport">
            <sqoop xmlns="uri:oozie:sqoop-action:0.2">
                <job-tracker>${jobTracker}</job-tracker>
                <name-node>${nameNode}</name-node>
                <configuration>
                    <property>
                        <name>mapred.compress.map.output</name>
                        <value>true</value>
                    </property>
                </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
            </sqoop>
            <ok to="end"/>
            <error to="fail"/>
        </action>

        <kill name="fail">
            <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>

        <end name="end"/>
    </workflow-app>

Nincsenek definiált hello munkafolyamat két műveleteket. hello start-tooaction van *RunHiveScript*. Hello művelet sikeresen lefutott, ha van-e a következő művelet hello *RunSqoopExport*.

hello RunHiveScript több változót tartalmaz. Ha az Azure PowerShell használatával a munkaállomáson a hello Oozie feladat elküldéséhez. át hello értékeket.

<table border = "1">
<tr><th>Munkafolyamat-változók</th><th>Leírás</th></tr>
<tr><td>${jobTracker}</td><td>Adja meg a Hadoop-feladat követő hello hello URL-CÍMÉT. Használjon <strong>jobtrackerhost:9010</strong> hdinsight 3.0-s és a 2.1-es verzióját.</td></tr>
<tr><td>${nameNode}</td><td>Megadja a hello Hadoop neve csomópont hello URL-CÍMÉT. Használjon hello alapértelmezett fájl rendszer címet, például <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
<tr><td>${queueName}</td><td>Adja meg a feladat hello hello várólistacímke elküldve. Használjon hello <strong>alapértelmezett</strong>.</td></tr>
</table>

<table border = "1">
<tr><th>Műveleti változó struktúra</th><th>Leírás</th></tr>
<tr><td>${hiveDataFolder}</td><td>Megadja a hello forráskönyvtár az hello Hive Create Table parancsot.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Hello FELÜLÍRÁSA INSERT utasítás hello kimeneti mappáját adja meg.</td></tr>
<tr><td>${hiveTableName}</td><td>Hello Hive tábla által hivatkozott hello log4j adatfájlok hello nevét adja meg.</td></tr>
</table>

<table border = "1">
<tr><th>Sqoop művelet változó</th><th>Leírás</th></tr>
<tr><td>${sqlDatabaseConnectionString}</td><td>Megadja a hello Azure SQL adatbázis-kapcsolati karakterlánc.</td></tr>
<tr><td>${sqlDatabaseTableName}</td><td>Megadja, ahol hello adatok exportált hello Azure SQL-adatbázistáblában szereplő.</td></tr>
<tr><td>${hiveOutputFolder}</td><td>Hello kimeneti mappáját hello utasítás Hive BESZÚRÁSA FELÜLÍRÁSÁHOZ adja meg. Ez a hello ugyanabban a mappában hello Sqoop export (Exportálás-dir).</td></tr>
</table>

Oozie munkafolyamat és a munkafolyamat-műveleteket használatával kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).

hello Hive művelet hello munkafolyamat meghívja a HiveQL-parancsfájlt. A parancsfájl három HiveQL utasításokat tartalmazza:

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. **hello DROP TABLE utasítás** törlések hello log4j Hive táblát, ha van ilyen.
2. **CREATE TABLE utasítás hello** log4j Hive külső tábla létrehozása, amely hello log4j naplófájl helye toohello mutat. hello mező határoló ",". hello alapértelmezett sor elválasztó karaktere "\n". A Hive külső tábla használt tooavoid hello adatfájl távolít el hello eredeti helyre, ha azt szeretné, toorun hello Oozie munkafolyamat több alkalommal.
3. **hello FELÜLÍRÁSA INSERT utasítás** hello log4j Hive tábla napló szintű típusonkénti hello előfordulások száma, és menti a hello kimeneti tooa blob az Azure Storage.

Három változók hello parancsfájl használatban van:

* ${hiveTableName}
* ${hiveDataFolder}
* ${hiveOutputFolder}

hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) HiveQL-parancsfájlt továbbítja ezeket az értékeket toothis futási időben.

A blob-tároló hello munkafolyamat fájl- és a HiveQL fájl hello tárolódnak.  PowerShell-parancsprogram, az oktatóanyag későbbi részében hello mindkét fájlok toohello alapértelmezett tárfiók másolja. 

## <a name="submit-oozie-jobs-using-powershell"></a>PowerShell-lel Oozie feladatok elküldéséhez
Az Azure PowerShell jelenleg nem biztosít semmilyen parancsmagok Oozie feladatok meghatározásához. Használhatja a hello **Invoke-RestMethod** parancsmag tooinvoke Oozie webszolgáltatások. hello Oozie webszolgáltatások API egy HTTP REST API-t JSON. Hello Oozie webszolgáltatások API kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).

Ebben a szakaszban a PowerShell parancsfájl hello hello a lépéseket követve hajtja végre:

1. Csatlakozás tooAzure.
2. Azure-erőforráscsoport létrehozása További információkért lásd: [használata Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).
3. Hozzon létre egy Azure SQL adatbázis-kiszolgáló, az Azure SQL-adatbázis és a két tábla. Ezek hello Sqoop művelet hello munkafolyamat által használt.
   
    hello táblázat neve *log4jLogCount*.
4. Hozzon létre egy HDInsight-fürt használt toorun Oozie feladatokat.
   
    tooexamine hello fürt, használhatja hello Azure-portálon vagy az Azure PowerShell.
5. Másolja a hello oozie munkafolyamat fájl- és hello HiveQL parancsfájl fájl toohello alapértelmezett fájlrendszert.
   
    Mindkét fájljai a következő nyilvános blobtárolóban.
   
   * Másolja a hello HiveQL parancsfájl (useoozie.hql) tooAzure Storage (wasb:///tutorials/useoozie/useoozie.hql).
   * Workflow.xml toowasb:///tutorials/useoozie/workflow.xml másolja.
   * Hello adatok fájl másolása (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.
6. Az Oozie feladat küldése
   
    tooexamine hello OOzie feladat eredményeinek, használja a Visual Studio vagy más eszközök tooconnect toohello Azure SQL Database.

Itt található hello parancsfájl.  A Windows PowerShell ISE hello parancsfájlt is futtathatja. Csak akkor kell tooconfigure hello első 7 változók.

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure subscription ID>"

    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"

    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # hello default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"

    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion

    #region - Create SQL database tables
    Write-Host "Creating hello log4jlogs table  ..." -ForegroundColor Green

    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()

    $conn.close()
    #endregion

    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

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
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - copy Oozie workflow and HiveQL files

    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green

    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force

    #validate hello copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml

    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql

    #endregion

    #region - copy hello sample.log file

    Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green

    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 

    #validate hello copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log

    #endregion

    #region - submit Oozie job

    $storageUri="wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"

    $oozieJobName = $namePrefix + "OozieJob"

    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

    <property>
        <name>nameNode</name>
        <value>$storageUrI</value>
    </property>

    <property>
        <name>jobTracker</name>
        <value>jobtrackerhost:9010</value>
    </property>

    <property>
        <name>queueName</name>
        <value>default</value>
    </property>

    <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
    </property>

    <property>
        <name>hiveScript</name>
        <value>$hiveScript</value>
    </property>

    <property>
        <name>hiveTableName</name>
        <value>$hiveTableName</value>
    </property>

    <property>
        <name>hiveDataFolder</name>
        <value>$hiveDataFolder</value>
    </property>

    <property>
        <name>hiveOutputFolder</name>
        <value>$hiveOutputFolder</value>
    </property>

    <property>
        <name>sqlDatabaseConnectionString</name>
        <value>&quot;$sqlDatabaseConnectionString&quot;</value>
    </property>

    <property>
        <name>sqlDatabaseTableName</name>
        <value>$SQLDatabaseTableName</value>
    </property>

    <property>
        <name>user.name</name>
        <value>$httpUserName</value>
    </property>

    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>

    </configuration>
    "@

    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."

    # create Oozie job
    Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."

    # start Oozie job
    Write-Host "Starting hello Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug

    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

    Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")

    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }

    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green

    #endregion


**futtatási toore hello oktatóanyag**

a munkafolyamat futtatási toore hello, törölnie kell a következő elemek hello:

* hello Hive parancsfájl kimeneti fájlja
* hello adatok hello log4jLogsCount táblázatban

Íme egy PowerShell-parancsfájlpélda közül választhat:

    $resourceGroupName = "<AzureResourceGroupName>"

    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat, és hogyan toorun egy Oozie feladat PowerShell használatával. toolearn több, tekintse meg a következő cikkek hello:

* [Időalapú Oozie-koordinátor használata a hdinsight eszközzel][hdinsight-oozie-coordinator-time]
* [Hadoop használatának megkezdésében a Hive HDInsight tooanalyze mobil kézibeszélő használatban][hdinsight-get-started]
* [Használhat Azure Blob Storage tárolót a hdinsight eszközzel][hdinsight-storage]
* [HDInsight a PowerShell felügyelete][hdinsight-admin-powershell]
* [Hdinsight Hadoop-feladatokat az adatok feltöltése][hdinsight-upload-data]
* [Sqoop használata a hadooppal a Hdinsightban][hdinsight-use-sqoop]
* [A Hive használata a hdinsight Hadoop][hdinsight-use-hive]
* [A Pig használata a HDInsight Hadoop][hdinsight-use-pig]
* [Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png  
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
