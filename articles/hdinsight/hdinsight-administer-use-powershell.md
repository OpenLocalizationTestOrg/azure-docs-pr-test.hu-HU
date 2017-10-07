---
title: a PowerShell - Azure hdinsight clusters aaaManage Hadoop |} Microsoft Docs
description: "Ismerje meg, hogyan tooperform felügyeleti feladatokat hello Azure PowerShell használatával hdinsight Hadoop-fürtök."
services: hdinsight
editor: cgronlun
manager: jhubbard
tags: azure-portal
author: mumian
documentationcenter: 
ms.assetid: bfdfa754-18e5-4ef9-b0d6-2dbdcebc0283
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 3df082d752fa8c703db82a54b82b740290af6729
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Hdinsight Hadoop-fürtök kezelése az Azure PowerShell használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése az Azure-ban a feladatok automatizálásához. Ebből a cikkből megtudhatja, hogyan toomanage Hadoop-fürtök az Azure HDInsight egy helyi Azure PowerShell-konzolon keresztül hello Windows PowerShell használja. HDInsight PowerShell-parancsmagok hello hello listájáért lásd: [HDInsight parancsmag-referencia][hdinsight-powershell-reference].

**Előfeltételek**

Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Ha telepítette az Azure PowerShell 0,9 verziója x, el kell távolítani egy újabb verzió telepítése előtt.

hello toocheck hello verziója PowerShell telepítve:

    Get-Module *azure*

toouninstall hello régebbi verziójú, a hello Vezérlőpult Programok és szolgáltatások futtatásához.

## <a name="create-clusters"></a>Fürtök létrehozása
Lásd: [fürtök létrehozása Linux-alapú hdinsight Azure PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Lista fürtök
Használja a következő parancs toolist hello összes fürt ebben az előfizetésben hello:

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a>Fürt megjelenítése
Használja a következő parancs tooshow adatok egy adott fürt ebben az előfizetésben hello hello:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a>Fürtök törlése
A következő parancs toodelete fürt hello használata:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

A fürt hello fürt tartalmazó erőforráscsoportot hello eltávolításával is törli. Vegye figyelembe, többek között az alapértelmezett tárfiók hello hello csoport minden hello erőforrásának törölni szeretné.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a>Fürtök méretezése
hello fürt skálázás funkció lehetővé teszi, hogy anélkül, hogy toore fut az Azure HDInsight fürt által használt feldolgozó csomópontok száma toochange hello-hello fürt létrehozása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt hello verziója, ellenőrizheti a hello tulajdonságlapján.  Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

a fürt a HDInsight által támogatott különböző típusú adatok csomópontok hello számának módosítása hello következményei:

* Hadoop

    Zökkenőmentesen növelheti hello adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg hello művelet van folyamatban. A méretezési művelet sikertelen szabályosan kezeli, így hello fürt mindig marad működőképes állapotban.

    A Hadoop fürtök adatok csomópontok száma hello csökkentésével átméretezi, ha néhány hello fürt hello szolgáltatás újraindul. Ennek hatására az összes futó és függőben lévő feladatok toofail művelet skálázás hello hello megvalósításának következő. Akkor is, azonban küldje el újra hello feladatok hello művelet végrehajtása után.
* HBase

    Zökkenőmentesen hozzáadása vagy eltávolítása a csomópontok tooyour HBase-fürtöt futtatása. A területi kiszolgálók hello skálázás művelet befejezése néhány percen belül automatikusan elosztását. Azonban Ön kézzel is eloszthatja hello területi kiszolgálók történő naplózásának révén a fürt és a következő parancsok parancssori ablakból futó hello toohello headnode:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm

    Zökkenőmentesen hozzáadása vagy eltávolítása adatok csomópontok tooyour Storm-fürt futása közben is. De hello skálázás művelet sikeres befejezése után kell toorebalance hello topológia.

    Kétféle módon valósítható meg újraelosztás:

  * A Storm webes felhasználói felület
  * Parancssori felület (CLI) eszköz

    Tekintse meg a toohello [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.

    HDInsight-fürt hello hello Storm webes felhasználói felület érhető el:

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Például hogyan toouse hello CLI parancssori toorebalance hello Storm-topológia:

        ## Reconfigure hello topology "mytopology" toouse 5 worker processes,
        ## hello spout "blue-spout" toouse 3 executors, and
        ## hello bolt "yellow-bolt" toouse 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

toochange hello Azure PowerShell, futtassa a parancsot követő ügyfélgépről hello használatával Hadoop-fürt mérete:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) HTTP-webszolgáltatások a következő hello rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Akkor is a visszavonási/grant hello hozzáférést. toorevoke:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

toogrant:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter hello Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter hello HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> Hello hozzáférés biztosítása/visszavonása, visszaállítja, hello fürt felhasználónevet és jelszót.
>
>

Ez hello portálon keresztül is elvégezhető. Lásd: [felügyeletéhez HDInsight használatával hello Azure-portálon][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-felhasználó hitelesítő adatainak frissítése
Hello azonos művelet [Grant/revoke HTTP access](#grant/revoke-access). Ha hello fürt hello HTTP hozzáféréssel rendelkezik, akkor kell először vonja vissza.  És új HTTP felhasználói hitelesítő adatokkal majd hello hozzáférést.

## <a name="find-hello-default-storage-account"></a>Hello alapértelmezett tárfiók keresése
a következő Powershell-parancsfájl hello bemutatja, hogyan tooget hello alapértelmezett tárfiók neve és hello alapértelmezett tárfiók kulcsa fürt.

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-hello-resource-group"></a>Hello erőforráscsoportban található
Hello erőforrás-kezelő módban mindegyik HDInsight-fürt tooan Azure erőforráscsoport tartozik.  toofind hello erőforráscsoport:

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a>Feladatok elküldéséhez
**toosubmit MapReduce-feladatok**

Lásd: [Windows-alapú hdinsight Hadoop-MapReduce futtatása minták](hdinsight-run-samples.md).

**toosubmit Hive-feladatok**

Lásd: [PowerShell használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-powershell.md).

**toosubmit Pig-feladatokhoz**

Lásd: [PowerShell használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-powershell.md).

**toosubmit Sqoop feladatok**

Lásd: [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).

**toosubmit Oozie feladatok**

Lásd: [Hadoop toodefine és futtatása egy munkafolyamat hdinsight használata Oozie](hdinsight-use-oozie.md).

## <a name="upload-data-tooazure-blob-storage"></a>Töltse fel az adatok tooAzure Blob-tároló
Lásd: [töltse fel az adatok tooHDInsight][hdinsight-upload-data].

## <a name="see-also"></a>Lásd még:
* [HDInsight parancsmag-referencia dokumentáció][hdinsight-powershell-reference]
* [HDInsight felügyelete hello Azure-portál használatával][hdinsight-admin-portal]
* [HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]
* [A HDInsight-fürtök létrehozása][hdinsight-provision]
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [Hadoop-feladatokat programozott módon küldhet][hdinsight-submit-jobs]
* [Azure HDInsight – első lépések][hdinsight-get-started]

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-provision-custom-options]: hdinsight-hadoop-provision-linux-clusters.md#configuration
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md

[hdinsight-admin-cli]: hdinsight-administer-use-command-line.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-flight]: hdinsight-analyze-flight-delay-data.md

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-ps-provision]: ./media/hdinsight-administer-use-powershell/HDI.PS.Provision.png
