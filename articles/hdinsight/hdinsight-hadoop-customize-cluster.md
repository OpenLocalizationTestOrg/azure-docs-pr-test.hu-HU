---
title: "aaaCustomize a HDInsight-fürtök használata parancsfájl-műveletek - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize HDInsight clusters parancsfájlművelet használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 3a63e216-4163-40c1-aa04-6b42fd0162ad
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 076fff23e016db47bc7e9963582a545ad638e691
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-windows-based-hdinsight-clusters-using-script-action"></a>Parancsfájlművelet Windows-alapú HDInsight-fürtök testreszabása
**Parancsfájl-művelet** lehet használt tooinvoke [egyéni parancsfájlok](hdinsight-hadoop-script-actions.md) hello Fürtlétrehozási folyamat a fürt további szoftver telepítése során.

a cikkben szereplő információkat hello tooWindows-alapú HDInsight-fürtök adott. Linux-alapú fürtök esetében lásd: [testreszabása Linux-alapú HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

További Azure Storage-fiókok ideértve például a HDInsight-fürtök testre szabható más módszerekkel is, számos, hello Hadoop módosítása konfigurációs fájljai (core-site.xml, hive-site.xml stb.), vagy hozzáadása a megosztott függvénytárak (például a Hive, az Oozie) be általános helyek hello fürtben. Ezek a testreszabások Azure PowerShell, hello Azure HDInsight .NET SDK használatával, vagy hello Azure-portálon. További információkért lásd: [Hadoop létrehozása a HDInsight-fürtök][hdinsight-provision-cluster].

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell-cli-and-dotnet-sdk.md)]

## <a name="script-action-in-hello-cluster-creation-process"></a>Hello fürtlétrehozás parancsfájlművelet
Parancsfájlművelet csak akkor használható, amíg hello folyamatának létrehozása folyamatban van egy fürt. hello alábbi ábrán látható parancsfájl hello létrehozási folyamata során kerül végrehajtásra:

![A HDInsight fürt testreszabási és fürt létrehozása során szakaszai][img-hdi-cluster-states]

Hello fürt hello parancsprogram futtatásakor kerül, hello **ClusterCustomization** szakaszban. Ebben a szakaszban a hello parancsfájl hello rendszer rendszergazdai fiók alatt fut, minden hello párhuzamos megadott hello fürt csomópontja, és teljes rendszergazdai jogosultságokat biztosít hello csomópontján.

> [!NOTE]
> Mivel a rendszergazdai jogosultságokkal rendelkezik a fürtcsomópontokon hello során a **ClusterCustomization** -szakaszban is használhat hello parancsfájl tooperform műveletek, például a szolgáltatások, beleértve a Hadoop-kapcsolatos szolgáltatások indítása és leállítása. Így hello parancsfájl részeként meg kell győződnie arról, hogy hello Ambari és egyéb Hadoop-kapcsolódó szolgáltatás megfelelően működik előtt hello parancsfájl befejezése után történik. Ezek a szolgáltatások szükségesek toosuccessfully hello állapotának és hello fürt állapotának megállapítása közben létrehozása folyamatban van. Ha módosítja a fürtön, ezeket a szolgáltatásokat érintő beállításra, hello súgófunkciókat által biztosított kell használnia. Súgófunkciókat kapcsolatos további információkért lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].
>
>

hello kimeneti és hello hibanaplókat hello parancsfájl hello alapértelmezett tárfiók hello fürt megadott vannak tárolva. hello naplófájljainak tárolása a hello nevű tábla **u < \cluster-name-fragment >< \time-stamp > setuplog**. Ezek a hello parancsfájl futtatása minden csomóponton hello (átjárócsomópont és feldolgozó csomópontokat) hello fürt származó összesített naplók.

Az egyes fürtökön fogadhatnak több Parancsfájlműveletek megadott vannak hello sorrendben definiálásra. Egy parancsfájlt is futott, hello átjárócsomópont, hello munkavégző csomópontokhoz vagy mindkettőt.

HDInsight biztosít több parancsfájlok tooinstall hello összetevők HDInsight-fürtök a következő:

| Név | Szkript |
| --- | --- |
| **Spark telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/sparkconfigactionv03/Spark-Installer-v03.ps1. Lásd: [telepítése és használata a HDInsight Spark-fürtök][hdinsight-install-spark]. |
| **R telepítéséhez** |https://hdiconfigactions.BLOB.Core.Windows.NET/rconfigactionv02/r-Installer-v02.ps1. Lásd: [telepítése és használata R HDInsight-fürtök][hdinsight-install-r]. |
| **Solr telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/solrconfigactionv01/solr-Installer-v01.ps1. Lásd: [telepítése és használata Solr a HDInsight-fürtök](hdinsight-hadoop-solr-install.md). |
| - **Giraph telepítése** |https://hdiconfigactions.BLOB.Core.Windows.NET/giraphconfigactionv01/giraph-Installer-v01.ps1. Lásd: [telepítése és használata Giraph a HDInsight-fürtök](hdinsight-hadoop-giraph-install.md). |
| **Hive-könyvtárakhoz előzetes betöltése** |https://hdiconfigactions.BLOB.Core.Windows.NET/setupcustomhivelibsv01/Setup-customhivelibs-v01.ps1. Lásd: [kódtárak hozzáadása Hive HDInsight-fürtök](hdinsight-hadoop-add-hive-libraries.md) |

## <a name="call-scripts-using-hello-azure-portal"></a>Hívás parancsfájlok hello Azure-portál használatával
**A hello Azure-portálon**

1. Indítsa el a fürt létrehozása a részben ismertetett módon [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).
2. A választható konfigurációs, hello **Parancsfájlműveletek** panelen kattintson a **parancsfájlművelet hozzáadása** tooprovide részleteinek hello parancsfájlművelet, alább látható módon:

    ![Használja a fürt parancsfájlművelet toocustomize](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "használata parancsfájlművelet toocustomize fürt")

    <table border='1'>
        <tr><th>Tulajdonság</th><th>Érték</th></tr>
        <tr><td>Név</td>
            <td>Adja meg a hello parancsfájlművelet nevét.</td></tr>
        <tr><td>A parancsfájl URI azonosítója</td>
            <td>Hello URI toohello parancsfájl megadásához, amely meghívott toocustomize hello fürt. s</td></tr>
        <tr><td>HEAD/munkavégző</td>
            <td>Adja meg a hello csomópontok (**Head** vagy **munkavégző**) mely hello testreszabási a parancsfájl futtatása.</b>.
        <tr><td>Paraméterek</td>
            <td>Adja meg a hello paraméterek hello parancsfájl által szükség esetén.</td></tr>
    </table>

    Nyomja le az ENTER tooadd több mint egy parancsfájl művelet tooinstall több összetevő hello fürtön.
3. Kattintson a **válasszon** toosave hello parancsprogram művelet konfigurációját, és folytassa a fürt létrehozása.

## <a name="call-scripts-using-azure-powershell"></a>Hívja meg az Azure PowerShell parancsfájlok
A következő PowerShell-parancsfájl bemutatja, hogyan tooinstall a Spark on Windows alapú HDInsight-fürthöz.  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" toolist IDs.

    $nameToken = "<Enter A Name Token>"  # hello token is use toocreate Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.

    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "<Enter a Password>"

    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    #############################################################
    # Connect tooAzure
    #############################################################

    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID

    #############################################################
    # Prepare hello dependent components
    #############################################################

    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

    # Create storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext

    #############################################################
    # Create cluster with ApacheSpark
    #############################################################

    # Specify hello configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey


    # Add a script action toohello cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `

    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


tooinstall egyéb szoftvereihez, szüksége lesz tooreplace hello parancsfájl hello parancsfájlban:

Amikor a rendszer kéri, adja meg hello hitelesítő hello fürt. Hello fürt létrehozása előtt több percet is igénybe vehet.

## <a name="call-scripts-using-net-sdk"></a>Hívás parancsfájlok .NET SDK használatával
hello következő minta bemutatja, hogyan tooinstall a Spark on Windows alapú HDInsight-fürthöz. tooinstall egyéb szoftvereihez, szüksége lesz tooreplace hello parancsfájl hello kódot.

**a Spark HDInsight-fürtök toocreate**

1. Hozzon létre egy C# konzolalkalmazást a Visual Studióban.
2. Hello Nuget Package Manager Console futtassa a következő parancs hello.

        Install-Package Microsoft.Rest.ClientRuntime.Azure.Authentication -Pre
        Install-Package Microsoft.Azure.Management.ResourceManager -Pre
        Install-Package Microsoft.Azure.Management.HDInsight
3. A következő using utasításokat a Program.cs fájlban hello használata hello:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        using Microsoft.Azure.Management.ResourceManager;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Rest;
        using Microsoft.Rest.Azure.Authentication;
4. Helyezze hello kódot hello osztály hello alábbira:

        private static HDInsightManagementClient _hdiManagementClient;

        // Replace with your AAD tenant ID if necessary
        private const string TenantId = UserTokenProvider.CommonTenantId;
        private const string SubscriptionId = "<Your Azure Subscription ID>";
        // This is hello GUID for hello PowerShell client. Used for interactive logins in this example.
        private const string ClientId = "1950a258-227b-4e31-a9cf-717495945fc2";
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumWorkerNodes = 2;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const string NewClusterType = "Hadoop";
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            // Authenticate and get a token
            var authToken = Authenticate(TenantId, ClientId, SubscriptionId);
            // Flag subscription for HDInsight, if it isn't already.
            EnableHDInsight(authToken);
            // Get an HDInsight management client
            _hdiManagementClient = new HDInsightManagementClient(authToken);

            CreateCluster();
        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumWorkerNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,
                DefaultStorageInfo = new AzureStorageInfo(ExistingStorageName, ExistingStorageKey, ExistingContainer),
                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }

        /// <summary>
        /// Authenticate tooan Azure subscription and retrieve an authentication token
        /// </summary>
        /// <param name="TenantId">hello AAD tenant ID</param>
        /// <param name="ClientId">hello AAD client ID</param>
        /// <param name="SubscriptionId">hello Azure subscription ID</param>
        /// <returns></returns>
        static TokenCloudCredentials Authenticate(string TenantId, string ClientId, string SubscriptionId)
        {
            var authContext = new AuthenticationContext("https://login.microsoftonline.com/" + TenantId);
            var tokenAuthResult = authContext.AcquireToken("https://management.core.windows.net/",
                ClientId,
                new Uri("urn:ietf:wg:oauth:2.0:oob"),
                PromptBehavior.Always,
                UserIdentifier.AnyUser);
            return new TokenCloudCredentials(SubscriptionId, tokenAuthResult.AccessToken);
        }
        /// <summary>
        /// Marks your subscription as one that can use HDInsight, if it has not already been marked as such.
        /// </summary>
        /// <remarks>This is essentially a one-time action; if you have already done something with HDInsight
        /// on your subscription, then this isn't needed at all and will do nothing.</remarks>
        /// <param name="authToken">An authentication token for your Azure subscription</param>
        static void EnableHDInsight(TokenCloudCredentials authToken)
        {
            // Create a client for hello Resource manager and set hello subscription ID
            var resourceManagementClient = new ResourceManagementClient(new TokenCredentials(authToken.Token));
            resourceManagementClient.SubscriptionId = SubscriptionId;
            // Register hello HDInsight provider
            var rpResult = resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        }
5. Nyomja le az **F5** toorun hello alkalmazás.

## <a name="support-for-open-source-software-used-on-hdinsight-clusters"></a>A HDInsight-fürtökön használt nyílt forráskódú szoftvereket támogatása
a Microsoft Azure HDInsight szolgáltatás hello egy rugalmas platform, amely lehetővé teszi az ökoszisztéma formátumú-e körül Hadoop nyílt forráskódú technológiák használatával toobuild big data-alkalmazások hello felhőben. A Microsoft Azure biztosít, általános támogatási nyílt forráskódú technológiák hello leírtaknak megfelelően **támogatja a hatókör** hello szakasza <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure támogatási gyakori Kérdéseinek webhelyre</a>. HDInsight szolgáltatás hello biztosít egy további néhány hello összetevőjét támogatása az alább ismertetett.

Nyílt forráskódú összetevők hello HDInsight szolgáltatás elérhető két típusa van:

* **A beépített összetevők** -ezeket az összetevőket a HDInsight-fürtök előre telepített, és alapfunkciókat hello fürt adja meg. YARN ResourceManager, hello Hive query language (HiveQL) és hello Mahout könyvtár például toothis kategóriába tartozik. A kiszolgálófürt-összetevők teljes listáját a érhető [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md) </a>.
* **Egyéni összetevők** -hello fürt felhasználóként, telepítése vagy használata előtt a munkaterhelésnek valamelyik összetevő a hello közösségi vagy Ön által létrehozott.

A beépített összetevők teljes mértékben támogatottak, és Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.

> [!WARNING]
> Hello HDInsight-fürt összetevői teljes mértékben támogatottak, és a Microsoft Support fog tooisolate segítségével, és a kapcsolódó toothese összetevők problémák megoldásához.
>
> Egyéni összetevők kap minden üzleti szempontból ésszerű támogatási toohelp meg toofurther hello problémával kapcsolatos hibaelhárítás elősegítéséhez. Hello probléma megoldását, vagy kérni a tooengage elérhető csatorna az hello megnyitja részletes segítséget, hogy a technológiát találhatók technológiák eredményezhet. Például nincsenek sok közösségi webhelyek használható, például: [MSDN fórum hdinsight](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), [http://stackoverflow.com](http://stackoverflow.com). Is Apache projektek rendelkezik projekt helyek [http://apache.org](http://apache.org), például: [Hadoop](http://hadoop.apache.org/), [Spark](http://spark.apache.org/).
>
>

HDInsight-szolgáltatás hello toouse egyéni összetevők számos lehetőséget biztosít. Függetlenül attól, milyen összetevő használt vagy hello fürt telepíteni hello azonos szintű támogatást vonatkozik. Az alábbiakban olvashat egy listát hello leggyakoribb módon, hogy a HDInsight-fürtök egyéni összetevők használható:

1. Feladat elküldése - Hadoop és egyéb feladatok végrehajtása, vagy használjon egyéni összetevők elküldött toohello tárolófürt is lehet.
2. Fürt testreszabási - fürt létrehozása során megadhatja további beállításokat és egyéni hello fürtcsomópontokon telepített összetevők.
3. Minták – a népszerű egyéni összetevők, a Microsoft és a mások által biztosított mintákat ezeket az összetevőket hogyan használható a hello HDInsight-fürtökön. Ezeket a mintákat támogatás nélkül van megadva.

## <a name="develop-script-action-scripts"></a>Parancsfájlművelet-parancsfájlok fejlesztése
Lásd: [parancsfájlművelet fejlesztése parancsfájlok a HDInsight][hdinsight-write-script].

## <a name="see-also"></a>Lásd még:
* [Hdinsight Hadoop-fürtök létrehozása] [ hdinsight-provision-cluster] hogyan toocreate egy HDInsight fürt más egyéni beállítások használatával kapcsolatos utasításokat tartalmazza.
* [A HDInsight parancsfájlművelet-parancsfájlok fejlesztése][hdinsight-write-script]
* [Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]
* [Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-install-r]
* [Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).
* [Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"
