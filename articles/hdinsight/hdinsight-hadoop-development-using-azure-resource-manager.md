---
title: "a HDInsight tools aaaMigrate tooAzure erőforrás-kezelő |} Microsoft Docs"
description: "Hogyan HDInsight-fürtök toomigrate tooAzure fejlesztői erőforrás-kezelő eszközei"
services: hdinsight
editor: cgronlun
manager: jhubbard
author: nitinme
documentationcenter: 
ms.assetid: 05efedb5-6456-4552-87ff-156d77fbe2e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: c087ae63d2544e5badae6be9c258f783aa92e2ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrating-tooazure-resource-manager-based-development-tools-for-hdinsight-clusters"></a>A HDInsight-fürtök áttelepítése tooAzure fejlesztési Resource Manager-alapú eszközök

A HDInsight-ból kivezettük való Azure Service Manager ASM-alapú eszközök hdinsight. Ha használta az Azure PowerShell, az Azure parancssori felület vagy hello HDInsight .NET SDK toowork HDInsight-fürtökkel, javasolt toouse hello Azure Resource Manager ARM-alapú verzióiban PowerShell parancssori felület és továbbítja a .NET SDK áll. Ez a cikk ismerteti a mutatók hogyan toomigrate toohello új ARM-alapú módszer. Megfelelő esetben ez a cikk is rámutat hello ASM és ARM megközelítések hdinsight hello különbségei.

> [!IMPORTANT]
> a címterület-kezelés hello támogatása PowerShell parancssori felület, és a .NET SDK nem küld a **2017. január 1.**.
> 
> 

## <a name="migrating-azure-cli-tooazure-resource-manager"></a>Áttelepítése az Azure parancssori felület tooAzure erőforrás-kezelő
hello Azure CLI most alapértelmezett tooAzure a Resource Manager (ARM) módban, kivéve, ha frissíti a korábbi telepítés; Ebben az esetben szüksége lehet toouse hello `azure config mode arm` tooswitch tooARM üzemmód.

a parancsok alapszintű hello adott hello Azure CLI toowork megadott HDInsight Azure Service Management (ASM) a rendszer hello azonos ARM; használatakor azonban bizonyos paraméterek és kapcsolók előfordulhat, hogy új neve lehet, és nincsenek sok új paraméter elérhető ARM használatakor. Például használhatja `azure hdinsight cluster create` toospecify hello Azure virtuális hálózat, amely egy fürt, létre kell hozni, vagy Hive- és Oozie metaadattárhoz.

A HDInsight Azure Resource Manageren keresztül használatához alapvető parancsok a következők:

* `azure hdinsight cluster create`-hoz létre egy új HDInsight-fürt
* `azure hdinsight cluster delete`– egy meglévő HDInsight-fürt törlése
* `azure hdinsight cluster show`– egy meglévő fürthöz kapcsolatos információk megjelenítése
* `azure hdinsight cluster list`– ismerteti az Azure-előfizetés a HDInsight-fürtök

Használjon hello `-h` tooinspect hello paraméterek és kapcsolók érhető el minden egyes parancsnál.

### <a name="new-commands"></a>Új parancsok
Elérhető az Azure Resource Manager új parancsok a következők:

* `azure hdinsight cluster resize`-dinamikusan módosítások hello hello fürt feldolgozó csomópontok száma
* `azure hdinsight cluster enable-http-access`-lehetővé teszi, hogy a HTTPs hozzáférés toohello fürtöt (az alapértelmezés)
* `azure hdinsight cluster disable-http-access`– HTTPs hozzáférés toohello fürt letiltása
* `azure hdinsight script-action`-parancsokat biztosít létrehozása vagy kezelése Parancsfájlműveletek egy fürtön
* `azure hdinsight config`-parancsok biztosít a konfigurációs fájl létrehozásakor használható hello `hdinsight cluster create` parancs tooprovide konfigurációs adatait.

### <a name="deprecated-commands"></a>Elavult parancsok
Ha hello `azure hdinsight job` parancsok toosubmit feladatok tooyour HDInsight-fürtjéhez, ezek nincsenek hello ARM-parancsok keresztül érhető el. Ha tooprogrammatically küldés feladatok tooHDInsight parancsfájlok, Ehelyett használjon hello REST API-k, a HDInsight által biztosított. A REST API-k használatával feladatok elküldésekor további információkért lásd: a következő dokumentumok hello.

* [Hadoop MapReduce-feladatok a HDInsight használata cURL használatával futtassa](hdinsight-hadoop-use-mapreduce-curl.md)
* [A Hadoop Hive-lekérdezések futtatása a HDInsight használata cURL használatával](hdinsight-hadoop-use-hive-curl.md)
* [Pig-feladatokhoz Hadoop on HDInsight használata cURL használatával futtassa](hdinsight-hadoop-use-pig-curl.md)

Más módokon toorun MapReduce olvashat, struktúra, és interaktív módon sertésfelmérés, lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md), [használata a hdinsight Hadoop Hive](hdinsight-use-hive.md), és [a Pig használata a hadooppal HDInsight](hdinsight-use-pig.md).

### <a name="examples"></a>Példák
**Fürt létrehozása**

* Régi parancs (ASM)-`azure hdinsight cluster create myhdicluster --location northeurope --osType linux --storageAccountName mystorage --storageAccountKey <storagekey> --storageContainer mycontainer --userName admin --password mypassword --sshUserName sshuser --sshPassword mypassword`
* Új parancs (ARM)-`azure hdinsight cluster create myhdicluster -g myresourcegroup --location northeurope --osType linux --clusterType hadoop --defaultStorageAccountName mystorage --defaultStorageAccountKey <storagekey> --defaultStorageContainer mycontainer --userName admin -password mypassword --sshUserName sshuser --sshPassword mypassword`

**A fürtök törlésével**

* Régi parancs (ASM)-`azure hdinsight cluster delete myhdicluster`
* Új parancs (ARM)-`azure hdinsight cluster delete mycluster -g myresourcegroup`

**Lista fürtök**

* Régi parancs (ASM)-`azure hdinsight cluster list`
* Új parancs (ARM)-`azure hdinsight cluster list`

> [!NOTE]
> A hello parancs megadásával hello erőforrás csoport használatával `-g` csak hello fürtök visszaadható hello megadott erőforráscsoportban.
> 
> 

**Fürt információ megjelenítése**

* Régi parancs (ASM)-`azure hdinsight cluster show myhdicluster`
* Új parancs (ARM)-`azure hdinsight cluster show myhdicluster -g myresourcegroup`

## <a name="migrating-azure-powershell-tooazure-resource-manager"></a>Áttelepítése az Azure PowerShell tooAzure erőforrás-kezelő
hello általános információk az Azure PowerShell hello Azure Resource Managerrel (ARM) módban található [az Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).

hello Azure PowerShell ARM parancsmagok telepített-mellé a címterület-kezelési parancsmagok hello lehet. hello két mód hello parancsmagjait elkülönítsék névvel.  hello ARM üzemmódban van *AzureRmHDInsight* a hello parancsmag neve túl összehasonlításával*AzureHDInsight* hello ASM módban.  Például *New-AzureRmHDInsightCluster* vs. *Új AzureHDInsightCluster*. Paraméterek és kapcsolók hírek nevük lehet, hogy legyen, és nincsenek sok új paraméter elérhető ARM használatakor.  Például több parancsmagok szükséges nevű új kapcsoló *- ResourceGroupName*. 

Hello HDInsight-parancsmagokat használhatja, csatlakozzon a tooyour Azure-fiókra, és az új erőforráscsoport létrehozása:

* Login-AzureRmAccount vagy [válasszon-AzureRmProfile](https://msdn.microsoft.com/library/mt619310.aspx). Lásd: [hitelesítéséhez az Azure Resource Manager egyszerű szolgáltatás](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Új-AzureRmResourceGroup](https://msdn.microsoft.com/library/mt603739.aspx)

### <a name="renamed-cmdlets"></a>Átnevezett parancsmagok
toolist hello HDInsight címterület-kezelési parancsmagok a Windows PowerShell-konzolon:

    help *azurermhdinsight*

hello következő táblázatban hello címterület-kezelési parancsmagok és a nevek hello ARM üzemmódban:

| Címterület-kezelési parancsmagok | ARM-parancsmagok |
| --- | --- |
| Add-AzureHDInsightConfigValues |[Adja hozzá AzureRmHDInsightConfigValues](https://msdn.microsoft.com/library/mt603530.aspx) |
| Add-AzureHDInsightMetastore |[Adja hozzá AzureRmHDInsightMetastore](https://msdn.microsoft.com/library/mt603670.aspx) |
| Add-AzureHDInsightScriptAction |[Adja hozzá AzureRmHDInsightScriptAction](https://msdn.microsoft.com/library/mt603527.aspx) |
| Add-AzureHDInsightStorage |[Adja hozzá AzureRmHDInsightStorage](https://msdn.microsoft.com/library/mt619445.aspx) |
| Get-AzureHDInsightCluster |[Get-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619371.aspx) |
| Get-AzureHDInsightJob |[Get-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603590.aspx) |
| Get-AzureHDInsightJobOutput |[Get-AzureRmHDInsightJobOutput](https://msdn.microsoft.com/library/mt603793.aspx) |
| Get-AzureHDInsightProperties |[Get-AzureRmHDInsightProperties](https://msdn.microsoft.com/library/mt603546.aspx) |
| Grant-AzureHDInsightHttpServicesAccess |[Támogatás-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619407.aspx) |
| Grant-AzureHdinsightRdpAccess |[Támogatás-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603717.aspx) |
| Invoke-AzureHDInsightHiveJob |[Invoke-AzureRmHDInsightHiveJob](https://msdn.microsoft.com/library/mt603593.aspx) |
| New-AzureHDInsightCluster |[Új AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619331.aspx) |
| New-AzureHDInsightClusterConfig |[Új AzureRmHDInsightClusterConfig](https://msdn.microsoft.com/library/mt603700.aspx) |
| New-AzureHDInsightHiveJobDefinition |[Új AzureRmHDInsightHiveJobDefinition](https://msdn.microsoft.com/library/mt619448.aspx) |
| New-AzureHDInsightMapReduceJobDefinition |[Új AzureRmHDInsightMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| New-AzureHDInsightPigJobDefinition |[Új AzureRmHDInsightPigJobDefinition](https://msdn.microsoft.com/library/mt603671.aspx) |
| New-AzureHDInsightSqoopJobDefinition |[Új AzureRmHDInsightSqoopJobDefinition](https://msdn.microsoft.com/library/mt608551.aspx) |
| New-AzureHDInsightStreamingMapReduceJobDefinition |[Új AzureRmHDInsightStreamingMapReduceJobDefinition](https://msdn.microsoft.com/library/mt603626.aspx) |
| Remove-AzureHDInsightCluster |[Remove-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619431.aspx) |
| Revoke-AzureHDInsightHttpServicesAccess |[A visszavonási-AzureRmHDInsightHttpServicesAccess](https://msdn.microsoft.com/library/mt619375.aspx) |
| Revoke-AzureHdinsightRdpAccess |[A visszavonási-AzureRmHDInsightRdpServicesAccess](https://msdn.microsoft.com/library/mt603523.aspx) |
| Set-AzureHDInsightClusterSize |[Set-AzureRmHDInsightClusterSize](https://msdn.microsoft.com/library/mt603513.aspx) |
| Set-AzureHDInsightDefaultStorage |[Set-AzureRmHDInsightDefaultStorage](https://msdn.microsoft.com/library/mt603486.aspx) |
| Start-AzureHDInsightJob |[Start-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603798.aspx) |
| Stop-AzureHDInsightJob |[STOP-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt619424.aspx) |
| Use-AzureHDInsightCluster |[Használjon-AzureRmHDInsightCluster](https://msdn.microsoft.com/library/mt619442.aspx) |
| Wait-AzureHDInsightJob |[Várjon, amíg-AzureRmHDInsightJob](https://msdn.microsoft.com/library/mt603834.aspx) |

### <a name="new-cmdlets"></a>Új parancsmagok
Az alábbiakban hello hello új parancsmagokat csak hello ARM módban érhető el. 

**A kapcsolódó parancsmagok parancsfájlművelet:**

* **Get-AzureRmHDInsightPersistedScriptAction**: lekérdezi hello megőrzött Parancsfájlműveletek fürt és időrendben sorolja fel azokat, vagy egy megadott megőrzött parancsfájl művelet lekérdezi a részletek. 
* **Get-AzureRmHDInsightScriptActionHistory**: hello parancsfájlművelet-előzmény lekérdezi a fürt fordított időrendben sorolja fel, és lekérdezi a korábban végrehajtott parancsfájlművelet részleteit. 
* **Remove-AzureRmHDInsightPersistedScriptAction**: egy megőrzött parancsfájl művelet eltávolítja a HDInsight-fürtöt.
* **Set-AzureRmHDInsightPersistedScriptAction**: állítja be a korábban végrehajtott parancsfájl művelet toobe egy megőrzött parancsfájl műveletet.
* **Küldje el AzureRmHDInsightScriptAction**: egy új parancsfájl művelet tooan Azure HDInsight-fürtöt ad meg. 

További használati információkért lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

**A kapcsolódó parancsmagok Clsuter identitás:**

* **Adja hozzá AzureRmHDInsightClusterIdentity**: hozzáadja a fürt identitását tooa konfigurációs objektum érdekében, hogy a hello HDInsight-fürt Azure Data Lake tárolja. Lásd: [HDInsight-fürtök létrehozása az Azure PowerShell használatával a Data Lake Store](../data-lake-store/data-lake-store-hdinsight-hadoop-use-powershell.md).

### <a name="examples"></a>Példák
**Fürt létrehozása**

Régi parancs (ASM): 

    New-AzureHDInsightCluster `
        -Name $clusterName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainerName $containerName `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -Credential $httpCredential `
        -SshCredential $sshCredential

Új parancs (ARM):

    New-AzureRmHDInsightCluster `
        -ClusterName $clusterName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $storageAccountKey `
        -DefaultStorageContainer $containerName  `
        -ClusterSizeInNodes 2 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.2" `
        -HttpCredential $httpcredentials `
        -SshCredential $sshCredentials


**Fürt törlése**

Régi parancs (ASM):

    Remove-AzureHDInsightCluster -name $clusterName 

Új parancs (ARM):

    Remove-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName 

**Lista fürt**

Régi parancs (ASM):

    Get-AzureHDInsightCluster

Új parancs (ARM):

    Get-AzureRmHDInsightCluster 

**Fürt megjelenítése**

Régi parancs (ASM):

    Get-AzureHDInsightCluster -Name $clusterName

Új parancs (ARM):

    Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -clusterName $clusterName


#### <a name="other-samples"></a>Más minták
* [A HDInsight-fürtök létrehozása](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
* [Küldje el a Hive-feladatok](hdinsight-hadoop-use-hive-powershell.md)
* [Küldje el a Pig-feladatokhoz](hdinsight-hadoop-use-pig-powershell.md)
* [Sqoop feladatok elküldéséhez](hdinsight-hadoop-use-sqoop-powershell.md)

## <a name="migrating-toohello-arm-based-hdinsight-net-sdk"></a>Áttelepítése toohello ARM-alapú HDInsight .NET SDK
hello Azure Szolgáltatáskezelés-alapú [(ASM) HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt416619.aspx) elavult. Javasoljuk, toouse áll hello Azure Resource Manager-alapú [(ARM) a HDInsight .NET SDK](https://msdn.microsoft.com/library/azure/mt271028.aspx). hello következő ASM-alapú HDInsight-csomagok elavulttá válnak.

* `Microsoft.WindowsAzure.Management.HDInsight`
* `Microsoft.Hadoop.Client`

Ez a szakasz ismerteti a mutatók toomore tooperform bizonyos feladatok hello SDK ARM-alapú.

| Hogyan... ARM-alapú HDInsight SDK használatával hello | Hivatkozások |
| --- | --- |
| .NET SDK használatával a HDInsight-fürtök létrehozása |Lásd: [HDInsight-fürtök létrehozása .NET SDK használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md) |
| A fürt parancsfájlművelet .NET SDK-val testreszabása |Lásd: [testreszabása HDInsight Linux clusters parancsfájlművelet használatával](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-script-action) |
| Alkalmazások interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti |Lásd: [.NET SDK használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-dotnet-sdk.md). Ebben a cikkben hello kódrészletet hello interaktív hitelesítési módszert használja. |
| Alkalmazások nem interaktív .NET SDK-val az Azure Active Directory használatával hitelesíti |Lásd: [a HDInsight nem interaktív alkalmazások létrehozása](hdinsight-create-non-interactive-authentication-dotnet-applications.md) |
| .NET SDK használatával Hive feladat elküldése |Lásd: [elküldeni a Hive-feladatok](hdinsight-hadoop-use-hive-dotnet-sdk.md) |
| Elküldeni a Pig feladatot .NET SDK használatával |Lásd: [elküldeni a Pig-feladatokhoz](hdinsight-hadoop-use-pig-dotnet-sdk.md) |
| .NET SDK használatával Sqoop feladat elküldése |Lásd: [nyújt Sqoop feladatok](hdinsight-hadoop-use-sqoop-dotnet-sdk.md) |
| Lista HDInsight-fürtök .NET SDK használatával |Lásd: [lista HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#list-clusters) |
| A HDInsight-fürtök .NET SDK használatával méretezése |Lásd: [méretezési HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#scale-clusters) |
| GRANT/revoke access tooHDInsight fürtök .NET SDK használatával |Lásd: [Grant/revoke access tooHDInsight fürtök](hdinsight-administer-use-dotnet-sdk.md#grantrevoke-access) |
| HTTP felhasználó hitelesítő adatait a HDInsight-fürtök .NET SDK használatával |Lásd: [frissítés HTTP felhasználói hitelesítő adatokat a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#update-http-user-credentials) |
| Hello alapértelmezett tárfiók keresése a HDInsight-fürtök .NET SDK használatával |Lásd: [hello alapértelmezett tárfiók keresése a HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#find-the-default-storage-account) |
| .NET SDK használatával a HDInsight-fürtök törlése |Lásd: [törlése HDInsight-fürtök](hdinsight-administer-use-dotnet-sdk.md#delete-clusters) |

### <a name="examples"></a>Példák
A következő példák a hogyan van egy művelet hello ARM-alapú SDK a hello ASM-alapú SDK és hello egyenértékű kódrészletet használatával történik.

**A fürt CRUD-ügyfél létrehozása**

* Régi parancs (ASM)
  
        //Certificate auth
        //This logs hello application in using a subscription administration certificate, which is not offered in Azure Resource Manager (ARM)
  
        const string subid = "454467d4-60ca-4dfd-a556-216eeeeeeee1";
        var cred = new HDInsightCertificateCredential(new Guid(subid), new X509Certificate2(@"path\to\certificate.cer"));
        var client = HDInsightClient.Connect(cred);
* Új parancs (ARM) (egyszerű szolgáltatás engedélyezése)
  
        //Service principal auth
        //This will log hello application in as itself, rather than on behalf of a specific user.
        //For details, including how tooset up hello application, see:
        //   https://azure.microsoft.com/en-us/documentation/articles/hdinsight-create-non-interactive-authentication-dotnet-applications/
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);
* Új parancs (ARM) (felhasználói engedélyezési)
  
        //User auth
        //This will log hello application in on behalf of hello user.
        //hello end-user will see a login popup.
  
        var authFactory = new AuthenticationFactory();
  
        var account = new AzureAccount { Type = AzureAccount.AccountType.User, Id = username };
  
        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
  
        var accessToken = authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto).AccessToken;
  
        var creds = new TokenCloudCredentials(subId.ToString(), accessToken);
  
        _hdiManagementClient = new HDInsightManagementClient(creds);

**Fürt létrehozása**

* Régi parancs (ASM)
  
        var clusterInfo = new ClusterCreateParameters
                    {
                        Name = dnsName,
                        DefaultStorageAccountKey = key,
                        DefaultStorageContainer = defaultStorageContainer,
                        DefaultStorageAccountName = storageAccountDnsName,
                        ClusterSizeInNodes = 1,
                        ClusterType = type,
                        Location = "West US",
                        UserName = "admin",
                        Password = "*******",
                        Version = version,
                        HeadNodeSize = NodeVMSize.Large,
                    };
        clusterInfo.CoreConfiguration.Add(new KeyValuePair<string, string>("config1", "value1"));
        client.CreateCluster(clusterInfo);
* Új parancs (ARM)
  
        var clusterCreateParameters = new ClusterCreateParameters
            {
                Location = "West US",
                ClusterType = "Hadoop",
                Version = "3.1",
                OSType = OSType.Linux,
                DefaultStorageAccountName = "mystorage.blob.core.windows.net",
                DefaultStorageAccountKey =
                    "O9EQvp3A3AjXq/W27rst1GQfLllhp0gUeiUUn2D8zX2lU3taiXSSfqkZlcPv+nQcYUxYw==",
                UserName = "hadoopuser",
                Password = "*******",
                HeadNodeSize = "ExtraLarge",
                RdpUsername = "hdirp",
                RdpPassword = ""*******",
                RdpAccessExpiry = new DateTime(2025, 3, 1),
                ClusterSizeInNodes = 5
            };
        var coreConfigs = new Dictionary<string, string> {{"config1", "value1"}};
        clusterCreateParameters.Configurations.Add(ConfigurationKey.CoreSite, coreConfigs);

**HTTP-hozzáférés engedélyezése**

* Régi parancs (ASM)
  
        client.EnableHttp(dnsName, "West US", "admin", "*******");
* Új parancs (ARM)
  
        var httpParams = new HttpSettingsParameters
        {
               HttpUserEnabled = true,
               HttpUsername = "admin",
               HttpPassword = "*******",
        };
        client.Clusters.ConfigureHttpSettings(resourceGroup, dnsname, httpParams);

**A fürtök törlésével**

* Régi parancs (ASM)
  
        client.DeleteCluster(dnsName);
* Új parancs (ARM)
  
        client.Clusters.Delete(resourceGroup, dnsname);

