---
title: "HDInsight-fürtök bootstrap – Azure aaaCustomize |} Microsoft Docs"
description: "Ismerje meg, hogyan toocustomize HDInsight clusters rendszerindítási használatával."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: ab2ebf0c-e961-4e95-8151-9724ee22d769
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 0029680fd1aa0e9e6aa9cdf667256c31b7ddc565
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-hdinsight-clusters-using-bootstrap"></a><span data-ttu-id="38268-103">Rendszerindítási HDInsight-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="38268-103">Customize HDInsight clusters using Bootstrap</span></span>

<span data-ttu-id="38268-104">Egyes esetekben kívánt tooconfigure hello konfigurációs fájlokat, többek között:</span><span class="sxs-lookup"><span data-stu-id="38268-104">Sometimes, you want tooconfigure hello configuration files, which include:</span></span>

* <span data-ttu-id="38268-105">clusterIdentity.xml</span><span class="sxs-lookup"><span data-stu-id="38268-105">clusterIdentity.xml</span></span>
* <span data-ttu-id="38268-106">Core-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-106">core-site.xml</span></span>
* <span data-ttu-id="38268-107">Gateway.XML</span><span class="sxs-lookup"><span data-stu-id="38268-107">gateway.xml</span></span>
* <span data-ttu-id="38268-108">a hbase-env.xml</span><span class="sxs-lookup"><span data-stu-id="38268-108">hbase-env.xml</span></span>
* <span data-ttu-id="38268-109">a hbase-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-109">hbase-site.xml</span></span>
* <span data-ttu-id="38268-110">hdfs-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-110">hdfs-site.xml</span></span>
* <span data-ttu-id="38268-111">Hive-env.xml</span><span class="sxs-lookup"><span data-stu-id="38268-111">hive-env.xml</span></span>
* <span data-ttu-id="38268-112">Hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-112">hive-site.xml</span></span>
* <span data-ttu-id="38268-113">mapred-hely</span><span class="sxs-lookup"><span data-stu-id="38268-113">mapred-site</span></span>
* <span data-ttu-id="38268-114">oozie-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-114">oozie-site.xml</span></span>
* <span data-ttu-id="38268-115">oozie-env.xml</span><span class="sxs-lookup"><span data-stu-id="38268-115">oozie-env.xml</span></span>
* <span data-ttu-id="38268-116">a Storm-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-116">storm-site.xml</span></span>
* <span data-ttu-id="38268-117">tez-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-117">tez-site.xml</span></span>
* <span data-ttu-id="38268-118">webhcat-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-118">webhcat-site.xml</span></span>
* <span data-ttu-id="38268-119">yarn-site.xml</span><span class="sxs-lookup"><span data-stu-id="38268-119">yarn-site.xml</span></span>

<span data-ttu-id="38268-120">Nincsenek a rendszerindítási három módszer toouse:</span><span class="sxs-lookup"><span data-stu-id="38268-120">There are three methods toouse bootstrap:</span></span>

* <span data-ttu-id="38268-121">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="38268-121">Use Azure PowerShell</span></span>
* <span data-ttu-id="38268-122">A .NET SDK használata</span><span class="sxs-lookup"><span data-stu-id="38268-122">Use .NET SDK</span></span>
* <span data-ttu-id="38268-123">Az Azure Resource Manager sablonjainak használata</span><span class="sxs-lookup"><span data-stu-id="38268-123">Use Azure Resource Manager template</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

<span data-ttu-id="38268-124">További összetevők telepítése HDInsight-fürt hello létrehozás közbeni információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="38268-124">For information on installing additional components on HDInsight cluster during hello creation time, see:</span></span>

* [<span data-ttu-id="38268-125">Script Action (Linux) HDInsight-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="38268-125">Customize HDInsight clusters using Script Action (Linux)</span></span>](hdinsight-hadoop-customize-cluster-linux.md)

## <a name="use-azure-powershell"></a><span data-ttu-id="38268-126">Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="38268-126">Use Azure PowerShell</span></span>
<span data-ttu-id="38268-127">a következő PowerShell-kódjába hello testreszabja a Hive-konfiguráció:</span><span class="sxs-lookup"><span data-stu-id="38268-127">hello following PowerShell code customizes a Hive configuration:</span></span>

    # hive-site.xml configuration
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $existingResourceGroupName `
        -ClusterName $clusterName `
        -Location $location `
        -ClusterSizeInNodes $clusterSizeInNodes `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.5" `
        -HttpCredential $httpCredential `
        -Config $config 

<span data-ttu-id="38268-128">PowerShell parancsfájl teljes működő található [függelék – A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span><span class="sxs-lookup"><span data-stu-id="38268-128">A complete working PowerShell script can be found in [Appendix-A](#hdinsight-hadoop-customize-cluster-bootstrap.md/appx-a:-powershell-sample).</span></span>

<span data-ttu-id="38268-129">**tooverify hello módosítása:**</span><span class="sxs-lookup"><span data-stu-id="38268-129">**tooverify hello change:**</span></span>

1. <span data-ttu-id="38268-130">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="38268-130">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="38268-131">Hello bal oldali menüben kattintson **a HDInsight-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="38268-131">From hello left menu, click **HDInsight clusters**.</span></span> <span data-ttu-id="38268-132">Ha nem látja, kattintson a **további szolgáltatások** első.</span><span class="sxs-lookup"><span data-stu-id="38268-132">If you don't see it, click **More services** first.</span></span>
3. <span data-ttu-id="38268-133">Kattintson az imént létrehozott hello PowerShell-parancsfájl használatával hello fürt.</span><span class="sxs-lookup"><span data-stu-id="38268-133">Click hello cluster you just created using hello PowerShell script.</span></span>
4. <span data-ttu-id="38268-134">Kattintson a **irányítópult** hello panel tooopen hello tetején a hello Ambari felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="38268-134">Click **Dashboard** from hello top of hello blade tooopen hello Ambari UI.</span></span>
5. <span data-ttu-id="38268-135">Kattintson a **Hive** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="38268-135">Click **Hive** from hello left menu.</span></span>
6. <span data-ttu-id="38268-136">Kattintson a **hiveserver2-n** a **összegzés**.</span><span class="sxs-lookup"><span data-stu-id="38268-136">Click **HiveServer2** from **Summary**.</span></span>
7. <span data-ttu-id="38268-137">Kattintson a hello **Configs** fülre.</span><span class="sxs-lookup"><span data-stu-id="38268-137">Click hello **Configs** tab.</span></span>
8. <span data-ttu-id="38268-138">Kattintson a **Hive** hello bal oldali menüből.</span><span class="sxs-lookup"><span data-stu-id="38268-138">Click **Hive** from hello left menu.</span></span>
9. <span data-ttu-id="38268-139">Kattintson a hello **speciális** fülre.</span><span class="sxs-lookup"><span data-stu-id="38268-139">Click hello **Advanced** tab.</span></span>
10. <span data-ttu-id="38268-140">Görgessen le, majd bontsa ki a **hive-hely speciális**.</span><span class="sxs-lookup"><span data-stu-id="38268-140">Scroll down and then expand **Advanced hive-site**.</span></span>
11. <span data-ttu-id="38268-141">Keressen **hive.metastore.client.socket.timeout** hello szakaszban.</span><span class="sxs-lookup"><span data-stu-id="38268-141">Look for **hive.metastore.client.socket.timeout** in hello section.</span></span>

<span data-ttu-id="38268-142">Néhány más konfigurációs fájlokat testreszabásáról további minták:</span><span class="sxs-lookup"><span data-stu-id="38268-142">Some more samples on customizing other configuration files:</span></span>

    # hdfs-site.xml configuration
    $HdfsConfigValues = @{ "dfs.blocksize"="64m" } #default is 128MB in HDI 3.0 and 256MB in HDI 2.1

    # core-site.xml configuration
    $CoreConfigValues = @{ "ipc.client.connect.max.retries"="60" } #default 50

    # mapred-site.xml configuration
    $MapRedConfigValues = @{ "mapreduce.task.timeout"="1200000" } #default 600000

    # oozie-site.xml configuration
    $OozieConfigValues = @{ "oozie.service.coord.normal.default.timeout"="150" }  # default 120

<span data-ttu-id="38268-143">További információkért lásd: című Azim Uddin blog [testreszabása a HDInsight-fürt létrehozása](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="38268-143">For more information, see Azim Uddin's blog titled [Customizing HDInsight Cluster creation](http://blogs.msdn.com/b/bigdatasupport/archive/2014/04/15/customizing-hdinsight-cluster-provisioning-via-powershell-and-net-sdk.aspx).</span></span>

## <a name="use-net-sdk"></a><span data-ttu-id="38268-144">A .NET SDK használata</span><span class="sxs-lookup"><span data-stu-id="38268-144">Use .NET SDK</span></span>
<span data-ttu-id="38268-145">Lásd: [létrehozása Linux-alapú fürtökön a Hdinsightban az hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span><span class="sxs-lookup"><span data-stu-id="38268-145">See [Create Linux-based clusters in HDInsight using hello .NET SDK](hdinsight-hadoop-create-linux-clusters-dotnet-sdk.md#use-bootstrap).</span></span>

## <a name="use-resource-manager-template"></a><span data-ttu-id="38268-146">Használja a Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="38268-146">Use Resource Manager template</span></span>
<span data-ttu-id="38268-147">A Resource Manager-sablon rendszerindítási is használhatja:</span><span class="sxs-lookup"><span data-stu-id="38268-147">You can use bootstrap in Resource Manager template:</span></span>

    "configurations": {
        …
        "hive-site": {
            "hive.metastore.client.connect.retry.delay": "5",
            "hive.execution.engine": "mr",
            "hive.security.authorization.manager": "org.apache.hadoop.hive.ql.security.authorization.DefaultHiveAuthorizationProvider"
        }
    }


![HDInsight Hadoop fürthöz bootstrap Azure Resource Manager sablon testreszabása](./media/hdinsight-hadoop-customize-cluster-bootstrap/hdinsight-customize-cluster-bootstrap-arm.png)

## <a name="see-also"></a><span data-ttu-id="38268-149">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="38268-149">See also</span></span>
* <span data-ttu-id="38268-150">[Hdinsight Hadoop-fürtök létrehozása] [ hdinsight-provision-cluster] hogyan toocreate egy HDInsight fürt más egyéni beállítások használatával kapcsolatos utasításokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="38268-150">[Create Hadoop clusters in HDInsight][hdinsight-provision-cluster] provides instructions on how toocreate an HDInsight cluster by using other custom options.</span></span>
* <span data-ttu-id="38268-151">[A HDInsight parancsfájlművelet-parancsfájlok fejlesztése][hdinsight-write-script]</span><span class="sxs-lookup"><span data-stu-id="38268-151">[Develop Script Action scripts for HDInsight][hdinsight-write-script]</span></span>
* <span data-ttu-id="38268-152">[Telepítse, és válassza a Spark on HDInsight-fürtök][hdinsight-install-spark]</span><span class="sxs-lookup"><span data-stu-id="38268-152">[Install and use Spark on HDInsight clusters][hdinsight-install-spark]</span></span>
* <span data-ttu-id="38268-153">[Telepítheti és használhatja a HDInsight-fürtök R][hdinsight-install-r]</span><span class="sxs-lookup"><span data-stu-id="38268-153">[Install and use R on HDInsight clusters][hdinsight-install-r]</span></span>
* <span data-ttu-id="38268-154">[Telepítheti és használhatja a HDInsight-fürtök Solr](hdinsight-hadoop-solr-install.md).</span><span class="sxs-lookup"><span data-stu-id="38268-154">[Install and use Solr on HDInsight clusters](hdinsight-hadoop-solr-install.md).</span></span>
* <span data-ttu-id="38268-155">[Telepítheti és használhatja a HDInsight-fürtök Giraph](hdinsight-hadoop-giraph-install.md).</span><span class="sxs-lookup"><span data-stu-id="38268-155">[Install and use Giraph on HDInsight clusters](hdinsight-hadoop-giraph-install.md).</span></span>

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-hadoop-provision-linux-clusters.md
[powershell-install-configure]: /powershell/azureps-cmdlets-docs


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Fürt létrehozása során szakaszból"

## <a name="appx-a-powershell-sample"></a><span data-ttu-id="38268-157">Appx-A: PowerShell-példa</span><span class="sxs-lookup"><span data-stu-id="38268-157">Appx-A: PowerShell sample</span></span>
<span data-ttu-id="38268-158">A PowerShell parancsfájl HDInsight-fürtöt hoz létre, és egy Hive-beállítás testreszabása:</span><span class="sxs-lookup"><span data-stu-id="38268-158">This PowerShell script creates an HDInsight cluster and customizes a Hive setting:</span></span>

    ####################################
    # Set these variables
    ####################################
    #region - used for creating Azure service names
    $nameToken = "<ENTER AN ALIAS>" 
    #endregion

    #region - cluster user accounts
    $httpUserName = "admin"  #HDInsight cluster username
    $httpPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"

    $sshUserName = "sshuser" #HDInsight ssh user name
    $sshPassword = "<ENTER A PASSWORD>" #"<Enter a Password>"
    #endregion

    ####################################
    # Service names and varialbes
    ####################################
    #region - service names
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

    $resourceGroupName = $namePrefix + "rg"
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName

    $location = "East US 2"
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    ####################################
    # Connect tooAzure
    ####################################
    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{Login-AzureRmAccount}
    #endregion

    #region - Create an HDInsight cluster
    ####################################
    # Create dependent components
    ####################################
    Write-Host "Creating a resource group ..." -ForegroundColor Green
    New-AzureRmResourceGroup `
        -Name  $resourceGroupName `
        -Location $location

    Write-Host "Creating hello default storage account and default blob container ..."  -ForegroundColor Green
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_GRS

    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageContext = New-AzureStorageContext `
                                    -StorageAccountName $defaultStorageAccountName `
                                    -StorageAccountKey $defaultStorageAccountKey
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageContext #use hello cluster name as hello container name

    ####################################
    # Create a configuration object
    ####################################
    $hiveConfigValues = @{ "hive.metastore.client.socket.timeout"="90" }

    $config = New-AzureRmHDInsightClusterConfig `
        | Set-AzureRmHDInsightDefaultStorage `
            -StorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
            -StorageAccountKey $defaultStorageAccountKey `
        | Add-AzureRmHDInsightConfigValues `
            -HiveSite $hiveConfigValues 

    ####################################
    # Create an HDInsight cluster
    ####################################
    $httpPW = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$httpPW)

    $sshPW = ConvertTo-SecureString -String $sshPassword -AsPlainText -Force
    $sshCredential = New-Object System.Management.Automation.PSCredential($sshUserName,$sshPW)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $hdinsightClusterName `
        -Location $location `
        -ClusterSizeInNodes 1 `
        -ClusterType Hadoop `
        -OSType Linux `
        -Version "3.5" `
        -HttpCredential $httpCredential `
        -SshCredential $sshCredential `
        -Config $config

    ####################################
    # Verify hello cluster
    ####################################
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName

    #endregion
