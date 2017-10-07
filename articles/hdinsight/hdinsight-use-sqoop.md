---
title: az Azure HDInsight (Hadoop) feladatok aaaRun Apache Sqoop |} Microsoft Docs
description: "Ismerje meg, hogyan egy munkaállomás toorun Sqoop az Azure PowerShell toouse importálása és exportálása egy Hadoop-fürt és az Azure SQL-adatbázis között."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 2fdcc6b7-6ad5-4397-a30b-e7e389b66c7a
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: bdac507704937d77921c9c13d70aa2434c7e3be4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-sqoop-with-hadoop-in-hdinsight"></a>Sqoop használata a hadooppal a Hdinsightban
[!INCLUDE [sqoop-selector](../../includes/hdinsight-selector-use-sqoop.md)]

Megtudhatja, hogyan toouse Sqoop HDInsight tooimport és exportálás a HDInsight-fürt és Azure SQL database vagy az SQL Server-adatbázis között.

Hadoop természetes téve a feldolgozása a strukturálatlan és félig strukturált adatok, például a naplókat, valamint fájlt, azonban a is lehet a szükséges strukturált tooprocess relációs adatbázisokban tárolt adatokat.

[Sqoop] [ sqoop-user-guide-1.4.4] van egy eszköz tootransfer adatokat Hadoop-fürtök és a relációs adatbázisok között. Használható a relációs adatbázis-kezelő rendszerének (RDBMS) tooimport adatokat például SQL Server, MySQL vagy hello Hadoop elosztott fájlrendszer (HDFS), az Oracle hello adatok a Hadoop MapReduce vagy a Hive, majd exportálja újra hello adatok egy RDBMS. Ebben az oktatóanyagban egy SQL Server adatbázist használ a relációs adatbázis.

A HDInsight-fürtökön támogatott Sqoop verzióiért lásd: [What's new in HDInsight által biztosított hello fürt verziók?][hdinsight-versions]

## <a name="understand-hello-scenario"></a>Hello forgatókönyv ismertetése

HDInsight-fürtök néhány adatot tartalmaz. A következő két minta hello használhatja:

* A log4j naplófájlt, amely itt található: */example/data/sample.log*. a következő naplók hello kinyert hello fájlt:
  
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
* Nevű Hive tábla *hivesampletable*, amely hivatkozások hello adatok fájlba */hive/warehouse/hivesampletable*. hello táblázat tartalmaz néhány mobileszköz adat. 
  
  | Mező | Adattípus |
  | --- | --- |
  | ClientID |Karakterlánc |
  | querytime |Karakterlánc |
  | piaci |Karakterlánc |
  | deviceplatform |Karakterlánc |
  | devicemake |Karakterlánc |
  | devicemodel |Karakterlánc |
  | state |Karakterlánc |
  | Ország |Karakterlánc |
  | querydwelltime |Dupla |
  | munkamenet-azonosító |bigint |
  | sessionpagevieworder |bigint |

Először exportálnia *sample.log* és *hivesampletable* toohello Azure SQL-adatbázist vagy kiszolgáló tooSQL, majd hello mobileszköz adat tartalmazó hello tábla importálása biztonsági tooHDInsight hello használatával következő elérési úton:

    /tutorials/usesqoop/importeddata

## <a name="create-cluster-and-sql-database"></a>Fürt és az SQL-adatbázis létrehozása
Ez a szakasz bemutatja, hogyan toocreate fürt SQL-adatbázis, hello SQL adatbázis-sémák és futó hello oktatóanyag használatára vonatkozó hello Azure-portál és az Azure Resource Manager-sablon. hello sablon található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-with-sql-database/). hello Resource Manager-sablon toodeploy hello tábla sémái tooSQL adatbázis bacpac csomag hívja.  a következő nyilvános blobtárolóban https://hditutorialdata.blob.core.windows.net/usesqoop/SqoopTutorial-2016-2-23-11-2.bacpac hello bacpac csomag található. Egy személyes tárolót toouse hello bacpac-fájlok, használja a következő értékek hello sablonban hello:
   
        "storageKeyType": "Primary",
        "storageKey": "<TheAzureStorageAccountKey>",

Ha jobban szeret toouse Azure PowerShell toocreate hello fürt és hello SQL-adatbázis, lásd: [függelék](#appendix-a---a-powershell-sample).

1. Kattintson a következő kép tooopen hello Azure-portálon a Resource Manager sablon hello.         
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-hdinsight-linux-with-sql-database%2Fazuredeploy.json" target="_blank"><img src="./media/hdinsight-use-sqoop/deploy-to-azure.png" alt="Deploy tooAzure"></a>
   

2. Adja meg a következő tulajdonságai hello:

    - **Előfizetés**: Adja meg az Azure-előfizetéshez.
    - **Erőforráscsoport**: hozzon létre egy új Azure-erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot.  Egy erőforráscsoport egy felügyeleti célból.  Objektumok tárolója.
    - **Hely**: Válasszon ki egy régiót.
    - **ClusterName**: hello Hadoop-fürt nevét adja meg.
    - **A fürt bejelentkezési nevet és jelszót**: hello alapértelmezett bejelentkezési név az admin.
    - **SSH-felhasználónév és -jelszó**.
    - **SQL adatbázis-bejelentkezési nevet és jelszót**.
    - **hely _artifacts**: hello alapértelmezett értéket használja, kivéve, ha szeretné, hogy toouse saját backpac fájl egy másik helyen.
    - **hely Sas tokent _artifacts**: hagyja üresen a mezőt.
    - **Bacpac Fájlnév**: hello alapértelmezett értéket használja, kivéve, ha szeretné, hogy toouse saját backpac fájl.
     
     a következő értékek hello szoftveresen kötött hello változók szakaszban:
     
     | Alapértelmezett tárfiók neve | <CluterName>tároló |
     | --- | --- |
     | Az Azure SQL adatbázis-kiszolgáló neve |<ClusterName>dbserver |
     | Az Azure SQL-adatbázis neve |<ClusterName>DB |
     
     Jegyezze fel ezeket az értékeket.  Már szükség hello oktatóanyag későbbi részében.

3. Kattintson **OK** toosave hello paraméterek.

4 a hello **egyéni központi telepítés** panelen kattintson **erőforráscsoport** legördülő mezőben, és kattintson **új** toocreate egy új erőforráscsoportot. hello erőforráscsoport egy olyan tároló, amely csoportosítja hello fürtöt, hello függő tárfiókot és egyéb kapcsolt erőforrásokat.

5. Kattintson a **Legal terms** (Jogi feltételek), majd a **Create** (Létrehozás) gombra.

6. Kattintson a **Create** (Létrehozás) gombra. Megjelenik egy új csempe jelenik meg Submitting deployment sablon központi telepítéshez. Körülbelül 20 percet toocreate hello fürt és az SQL-adatbázis vesz igénybe.

Ha úgy dönt, hogy toouse meglévő Azure SQL adatbázis vagy a Microsoft SQL Server

* **Az Azure SQL adatbázis**: hello Azure SQL adatbázis-kiszolgáló tooallow hozzáférési egy tűzfalszabályt konfigurálnia kell a munkaállomáson. Egy Azure SQL-adatbázis létrehozása, és hello tűzfal konfigurálásával kapcsolatos útmutatásért lásd: [Azure SQL-adatbázis használatának első][sqldatabase-get-started]. 
  
  > [!NOTE]
  > Alapértelmezés szerint az Azure SQL adatbázis Azure-szolgáltatások, például az Azure HDInsight kapcsolatokat engedélyez. Ha a tűzfal beállítás le van tiltva, akkor kell-e tooenable azt hello Azure-portálon. További információk az Azure SQL-adatbázis létrehozása és a tűzfalszabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-create-configue].
  > 
  > 
* **SQL Server**: Ha a HDInsight-fürt hello ugyanazt a virtuális hálózatot az Azure SQL-kiszolgálóként, lépésekkel hello Ez a cikk tooimport és exportálási adatok tooa SQL Server adatbázisban.
  
  > [!NOTE]
  > HDInsight támogatja a csak helyalapú virtuális hálózatokat, és jelenleg nem működik az affinitáscsoport-alapú virtuális hálózatokon.
  > 
  > 
  
  * toocreate és virtuális hálózat konfigurálása, lásd: [hello Azure-portál virtuális hálózat létrehozása](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).
    
    * SQL Server használatakor az adatközpontban található konfigurálnia kell a virtuális hálózatban hello *pont-pont* vagy *pont-pont*.
      
      > [!NOTE]
      > A **pont-pont** virtuális hálózatok, az SQL Server futnia kell az hello VPN-ügyfél konfigurációs alkalmazás, amely elérhető a hello **irányítópult** az Azure-beli virtuális hálózat konfigurációját.
      > 
      > 
    * Használatakor az SQL Server Azure virtuális géphez, virtuális hálózati konfigurációt használható, ha hello virtuális gépet üzemeltető SQL Server hello tagja HDInsight megegyező virtuális hálózatban.
  * a virtuális hálózaton, HDInsight-fürtök toocreate lásd: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md)
    
    > [!NOTE]
    > SQL Server hitelesítési is lehetővé kell tenni. Bejelentkezési toocomplete hello ebben a cikkben ismertetett visszaállítási lépésekkel SQL-kiszolgálót kell használnia.
    > 
    > 

## <a name="run-sqoop-jobs"></a>Sqoop feladatok futtatása
HDInsight Sqoop feladatok futtatásához számos módszer használatával. Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.

| **Ezzel** Ha azt szeretné... | .. .an **interaktív** rendszerhéj | ... **kötegelt** feldolgozása | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél operációs rendszer** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-use-sqoop-mac-linux.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X vagy Windows |
| [.NET SDK a Hadoophoz](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |&nbsp; |✔ |Linux- vagy Windows |(Egyelőre) Windows |
| [Azure PowerShell](hdinsight-hadoop-use-sqoop-powershell.md) |&nbsp; |✔ |Linux- vagy Windows |Windows |

## <a name="limitations"></a>Korlátozások
* Tömeges export - a Linux-alapú HDInsight, hello Sqoop használt tooexport adatok tooMicrosoft SQL Server vagy az Azure SQL Database jelenleg nem támogatja a tömeges beszúrások.
* Kötegelés – a Linux-alapú hdinsight eszközzel, hello használatakor `-batch` Beszúrások végrehajtásakor kapcsoló, a Sqoop hajt végre több Beszúrások hello beszúrási műveletek kötegelése helyett.

## <a name="next-steps"></a>Következő lépések
Most megtanulta, hogyan toouse Sqoop. toolearn több, lásd:

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Oozie használata a HDInsight][hdinsight-use-oozie]: egy Oozie munkafolyamat használja Sqoop műveletét.
* [HDInsight eszközzel repülési késleltetés adatok elemzése][hdinsight-analyze-flight-data]: tooanalyze felhőszolgáltató közötti átviteléhez használja Hive késleltetés az adatok, és majd a Sqoop tooexport tooan Azure SQL-adatbázis használata.
* [Töltse fel az adatok tooHDInsight][hdinsight-upload-data]: található adatok tooHDInsight vagy az Azure Blob storage feltöltése más módszerrel.

## <a name="appendix-a---a-powershell-sample"></a>A melléklet – PowerShell-példa
hello PowerShell-példa hello a következő lépéseket végzi el:

1. Csatlakozás tooAzure.
2. Azure-erőforráscsoport létrehozása További információkért lásd: [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md)
3. Hozzon létre egy Azure SQL adatbázis-kiszolgáló, az Azure SQL-adatbázis és a két tábla. 
   
    Ha ehelyett SQL Servert használja, használja a következő utasítások toocreate hello táblák hello:
   
        CREATE TABLE [dbo].[log4jlogs](
         [t1] [nvarchar](50),
         [t2] [nvarchar](50),
         [t3] [nvarchar](50),
         [t4] [nvarchar](50),
         [t5] [nvarchar](50),
         [t6] [nvarchar](50),
         [t7] [nvarchar](50))
   
        CREATE TABLE [dbo].[mobiledata](
         [clientid] [nvarchar](50),
         [querytime] [nvarchar](50),
         [market] [nvarchar](50),
         [deviceplatform] [nvarchar](50),
         [devicemake] [nvarchar](50),
         [devicemodel] [nvarchar](50),
         [state] [nvarchar](50),
         [country] [nvarchar](50),
         [querydwelltime] [float],
         [sessionid] [bigint],
         [sessionpagevieworder][bigint])
   
    hello legegyszerűbb módja tooexamine hello adatbázis és a táblák nem toouse Visual Studio. hello adatbázis és adatbázis-kiszolgáló hello megvizsgálhatók hello Azure-portál használatával.
4. HDInsight-fürtök létrehozása.
   
    tooexamine hello fürt, használhatja hello Azure-portálon vagy az Azure PowerShell.
5. Előre feldolgozzák a hello forrásadatfájlok.
   
    Ebben az oktatóanyagban kell exportálnia a log4j naplófájl (tagolt fájl), és a Hive tábla tooan Azure SQL-adatbázis. hello tagolt fájl neve */example/data/sample.log*. Hello az oktatóanyagban a korábbi látott néhány minták log4j naplók. Hello naplófájlban van néhány üres sorok és az egyes sorok hasonló toothese:
   
        java.lang.Exception: 2012-02-03 20:11:35 SampleClass2 [FATAL] unrecoverable system problem at id 609774657
            at com.osa.mocklogger.MockLogger$2.run(MockLogger.java:83)
   
    Ennek megfelelően működik, ezeket az adatokat használó más példák, de azt előtt el kell távolítani ezeket a kivételeket azt importálnia hello Azure SQL database vagy az SQL Server. Sqoop exportálás sikertelen lesz, ha van egy üres karakterlánc vagy egy kevesebb sort hello Azure SQL adatbázis táblázatban definiált mezők hello száma elemet. hello log4jlogs táblához 7 karakterlánc típusú mezőket.
   
    Ez az eljárás létrehoz egy új fájlt hello fürt: tutorials/usesqoop/data/sample.log. tooexamine hello módosított adatfájlt, használhat hello Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell. [Ismerkedés a HDInsight] [ hdinsight-get-started] minta az Azure PowerShell toodownload egy fájl segítségével, és megjeleníti a hello fájl tartalma kódot tartalmaz.
6. Exportálja az adatokat fájl toohello Azure SQL-adatbázis.
   
    hello forrásfájl tutorials/usesqoop/data/sample.log. ahol hello adatok az exportált toois hello tábla log4jlogs nevezik.
   
   > [!NOTE]
   > Eltérő kapcsolati karakterlánc információt ebben a szakaszban található lépéseket hello Azure SQL-adatbázis, vagy az SQL Server kell működnie. Ezeket a lépéseket teszteltük hello konfiguráció a következő használatával:
   > 
   > * **Azure-beli virtuális hálózat pont-hely konfigurációs**: egy virtuális hálózathoz csatlakozó hello HDInsight fürt tooa SQL Server egy privát adatközpontban. Lásd: [pont-pont VPN konfigurálásához a kezelési portál hello](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.
   > * **Az Azure HDInsight 3.1**: lásd: [létrehozása Hadoop-fürtök a Hdinsightban egyéni beállításokkal](hdinsight-hadoop-provision-linux-clusters.md) információ a fürt létrehozása a virtuális hálózaton.
   > * **Az SQL Server 2014**: tooallow hitelesítési és futó hello VPN ügyfél konfigurációs csomag tooconnect biztonságosan konfigurált virtuális hálózati toohello.
   > 
   > 
7. Exportálja a Hive tábla toohello Azure SQL-adatbázis.
8. Importálja a hello mobiledata tábla toohello HDInsight-fürthöz.
   
    tooexamine hello módosított adatfájlt, használhat hello Azure-portálon, az Azure Storage explorer eszköz vagy az Azure PowerShell.  [Ismerkedés a HDInsight] [ hdinsight-get-started] Azure PowerShell toodownload fájl használatáról sample és hello fájl tartalom megjelenítése egy kódot.

### <a name="hello-powershell-sample"></a>hello PowerShell-példa
    # Prepare an Azure SQL database toobe used by hello Sqoop tutorial

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure Subscription ID>"

    $sqlDatabaseLogin = "<Enter a SQL Database Login name>" #SQL Database server login
    $sqlDatabasePassword = "<Enter a Password>"

    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<Enter a Password>"

    # used for creating Azure service names
    $nameToken = "<Enter an alias>" 
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

    # Used for creating tables and clustered indexes
    $cmdCreateLog4jTable = "CREATE TABLE [dbo].[log4jlogs](
        [t1] [nvarchar](50),
        [t2] [nvarchar](50),
        [t3] [nvarchar](50),
        [t4] [nvarchar](50),
        [t5] [nvarchar](50),
        [t6] [nvarchar](50),
        [t7] [nvarchar](50))"

    $cmdCreateLog4jClusteredIndex = "CREATE CLUSTERED INDEX log4jlogs_clustered_index on log4jlogs(t1)"

    $cmdCreateMobileTable = " CREATE TABLE [dbo].[mobiledata](
    [clientid] [nvarchar](50),
    [querytime] [nvarchar](50),
    [market] [nvarchar](50),
    [deviceplatform] [nvarchar](50),
    [devicemake] [nvarchar](50),
    [devicemodel] [nvarchar](50),
    [state] [nvarchar](50),
    [country] [nvarchar](50),
    [querydwelltime] [float],
    [sessionid] [bigint],
    [sessionpagevieworder][bigint])"

    $cmdCreateMobileDataClusteredIndex = "CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(clientid)"

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
    catch{Login-AzureRmAccount}
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
        $credential = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $credential `
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
    Write-Host "`nCreating an Azure SQL database ..." -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }

    #endregion

    #region - Create tables
    Write-Host "Creating hello log4jlogs table and hello mobiledata table ..." -ForegroundColor Green

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jTable
    $ret = $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateLog4jClusteredIndex
    $cmd.ExecuteNonQuery()

    # Create hello mobiledata table and index
    $cmd.CommandText = $cmdCreateMobileTable
    $cmd.ExecuteNonQuery()
    $cmd.CommandText = $cmdCreateMobileDataClusteredIndex
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

    #region - pre-process hello source file

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    # This procedure creates a new file with $destBlobName
    $sourceBlobName = "example/data/sample.log"
    $destBlobName = "tutorials/usesqoop/data/sample.log"

    # Define hello connection string
    $storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=$defaultStorageAccountName;AccountKey=$defaultStorageAccountKey"

    # Create block blob objects referencing hello source and destination blob.
    $storageAccount = [Microsoft.WindowsAzure.Storage.CloudStorageAccount]::Parse($storageConnectionString)
    $storageClient = $storageAccount.CreateCloudBlobClient();
    $storageContainer = $storageClient.GetContainerReference($defaultBlobContainerName)
    $sourceBlob = $storageContainer.GetBlockBlobReference($sourceBlobName)
    $destBlob = $storageContainer.GetBlockBlobReference($destBlobName)

    # Define a MemoryStream and a StreamReader for reading from hello source file
    $stream = New-Object System.IO.MemoryStream
    $stream = $sourceBlob.OpenRead()
    $sReader = New-Object System.IO.StreamReader($stream)

    # Define a MemoryStream and a StreamWriter for writing into hello destination file
    $memStream = New-Object System.IO.MemoryStream
    $writeStream = New-Object System.IO.StreamWriter $memStream

    # Pre-process hello source blob
    $exString = "java.lang.Exception:"
    while(-Not $sReader.EndOfStream){
        $line = $sReader.ReadLine()
        $split = $line.Split(" ")

        # remove hello "java.lang.Exception" from hello first element of hello array
        # for example: java.lang.Exception: 2012-02-03 19:11:02 SampleClass8 [WARN] problem finding id 153454612
        if ($split[0] -eq $exString){
            #create a new ArrayList tooremove $split[0]
            $newArray = [System.Collections.ArrayList] $split
            $newArray.Remove($exString)

            # update $split and $line
            $split = $newArray
            $line = $newArray -join(" ")
        }

        # remove hello lines that has less than 7 elements
        if ($split.count -ge 7){
            write-host $line
            $writeStream.WriteLine($line)
        }
    }

    # Write toohello destination blob
    $writeStream.Flush()
    $memStream.Seek(0, "Begin")
    $destBlob.UploadFromStream($memStream)

    #endregion

    #region - export a log file from hello cluster toohello SQL database

    Write-Host "Preprocessing hello source file ..." -ForegroundColor Green

    $tableName_log4j = "log4jlogs"

    # Connection string for Azure SQL Database.
    # Comment if using SQL Server
    $connectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"
    # Connection string for SQL Server.
    # Uncomment if using SQL Server.
    #$connectionString = "jdbc:sqlserver://$sqlDatabaseServerName;user=$sqlDatabaseLogin;password=$sqlDatabasePassword;database=$sqlDatabaseName"

    $exportDir_log4j = "/tutorials/usesqoop/data"

    # Submit a Sqoop job
    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_log4j --export-dir $exportDir_log4j --input-fields-terminated-by \0x20 -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose
    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardError
    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName -DefaultStorageAccountName $defaultStorageAccountName -DefaultStorageAccountKey $defaultStorageAccountKey -DefaultContainer $defaultBlobContainerName -HttpCredential $httpCredential -JobId $sqoopJob.JobId -DisplayOutputType StandardOutput

    #endregion

    #region - export a Hive table

    $tableName_mobile = "mobiledata"
    $exportDir_mobile = "/hive/warehouse/hivesampletable"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "export --connect $connectionString --table $tableName_mobile --export-dir $exportDir_mobile --fields-terminated-by \t -m 1"
    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion

    #region - import a database

    $targetDir_mobile = "/tutorials/usesqoop/importeddata/"

    $sqoopDef = New-AzureRmHDInsightSqoopJobDefinition `
        -Command "import --connect $connectionString --table $tableName_mobile --target-dir $targetDir_mobile --fields-terminated-by \t --lines-terminated-by \n -m 1"

    $sqoopJob = Start-AzureRmHDInsightJob `
                    -ClusterName $hdinsightClusterName `
                    -HttpCredential $httpCredential `
                    -JobDefinition $sqoopDef #-Debug -Verbose

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId

    Write-Host "Standard Error" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardError

    Write-Host "Standard Output" -BackgroundColor Green
    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -DefaultStorageAccountName $defaultStorageAccountName `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultBlobContainerName `
        -HttpCredential $httpCredential `
        -JobId $sqoopJob.JobId `
        -DisplayOutputType StandardOutput

    #endregion



[azure-management-portal]: https://portal.azure.com/

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-storage]: ../hdinsight-hadoop-use-blob-storage.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md
[sqldatabase-create-configue]: ../sql-database/sql-database-get-started.md

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-install]: /powershell/azureps-cmdlets-docs
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[sqoop-user-guide-1.4.4]: https://sqoop.apache.org/docs/1.4.4/SqoopUserGuide.html
