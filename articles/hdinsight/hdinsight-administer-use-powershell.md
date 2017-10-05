---
title: "A PowerShell - Azure hdinsight Hadoop-fürtök kezelése |} Microsoft Docs"
description: "Útmutató az Azure PowerShell hdinsight Hadoop-fürtök felügyeleti feladatokat hajthat végre."
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
ms.openlocfilehash: c47dabd7c4aa4ba0be08c419989e536711f03677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-by-using-azure-powershell"></a>Hdinsight Hadoop-fürtök kezelése az Azure PowerShell használatával
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti a munkaterhelések az Azure-ban, és szabályozhatja. Ön ebből a cikkből megtudhatja, hogyan Azure hdinsight Hadoop-fürtök kezelése a helyi Azure PowerShell-konzolban révén Windows PowerShell használatával. A HDInsight PowerShell-parancsmagok listáját lásd: [HDInsight parancsmag-referencia][hdinsight-powershell-reference].

**Előfeltételek**

A cikk elkezdéséhez az alábbiakkal kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

## <a name="install-azure-powershell"></a>Az Azure PowerShell telepítése
[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

Ha telepítette az Azure PowerShell 0,9 verziója x, el kell távolítani egy újabb verzió telepítése előtt.

A telepített PowerShell verziójának ellenőrzése:

    Get-Module *azure*

Távolítsa el a régebbi verziót, hogy futtassa a Vezérlőpult Programok és szolgáltatások.

## <a name="create-clusters"></a>Fürtök létrehozása
Lásd: [fürtök létrehozása Linux-alapú hdinsight Azure PowerShell használatával](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)

## <a name="list-clusters"></a>Lista fürtök
Használja a következő parancsot a jelenlegi előfizetés az összes fürt listázásához:

    Get-AzureRmHDInsightCluster

## <a name="show-cluster"></a>Fürt megjelenítése
A következő paranccsal egy adott fürt részleteinek megjelenítése az aktuális előfizetésben:

    Get-AzureRmHDInsightCluster -ClusterName <Cluster Name>

## <a name="delete-clusters"></a>Fürtök törlése
Az alábbi parancs segítségével törölheti a fürtöt:

    Remove-AzureRmHDInsightCluster -ClusterName <Cluster Name>

A fürt a fürt tartalmazó erőforráscsoportot eltávolításával is törli. Vegye figyelembe, többek között az alapértelmezett tárfiók csoportban található összes erőforrást törölni szeretné.

    Remove-AzureRmResourceGroup -Name <Resource Group Name>

## <a name="scale-clusters"></a>Fürtök méretezése
A fürt skálázás funkciót lehetővé teszi, hogy anélkül, hogy újra létre kell hoznia a fürt fut az Azure HDInsight fürt által használt feldolgozó csomópontok számának módosítása.

> [!NOTE]
> Csak verzió 3.1.3 hdinsight clusters vagy annál magasabb támogatottak. Ha biztos benne, hogy a fürt verzióját, a Tulajdonságok lapján ellenőrizheti.  Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
>
>

A fürt a HDInsight által támogatott különböző típusú adatok csomópontok számának módosítása következményei:

* Hadoop

    Zökkenőmentesen növelheti adhatja meg, hogy minden folyamatban lévő vagy a futó feladatok befolyásolása nélkül fut egy Hadoop-fürt feldolgozó csomópontjainak számát. Új feladatokat is küldheti el, amíg a művelet folyamatban van. A méretezési művelet sikertelen szabályosan kezeli, hogy a fürt mindig működőképes állapotban marad.

    A Hadoop fürtök adatok csomópontok számának csökkentésével átméretezi, ha néhány, a fürt a szolgáltatások újraindításáig. Ennek hatására a összes futó és függőben lévő feladatok meghiúsulhatnak, a méretezési művelet befejezését. Akkor is, azonban küldje el újra a feladatok a művelet végrehajtása után.
* HBase

    Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához a HBase-fürtöt a futtatása. Területi kiszolgálók automatikus elosztását a méretezési művelet befejezését néhány percen belül. Azonban Ön is manuálisan is elosztása a regionális kiszolgálók fürt headnode való bejelentkezés, és futtatja a következő parancsokat egy parancssori ablakot:

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer
* Storm

    Akkor is zökkenőmentesen csomópontok hozzáadásához és eltávolításához adatok Storm fürthöz való futtatása során. De a méretezési művelet sikeres befejezését követően szüksége lesz a topológia egyensúlyba.

    Kétféle módon valósítható meg újraelosztás:

  * A Storm webes felhasználói felület
  * Parancssori felület (CLI) eszköz

    Tekintse meg a [alatt futó Apache Storm-dokumentáció](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) további részleteket.

    A Storm webes felhasználói felület érhető el a HDInsight-fürt:

    ![A HDInsight alatt futó storm méretezési egyensúlyozza ki újra](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

    Íme egy példa a CLI parancs használata a Storm-topológia egyensúlyba:

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors
        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10

Ha módosítani szeretné a Hadoop-fürt mérete Azure PowerShell használatával, a következő parancsot a egy ügyfélszámítógépen:

    Set-AzureRmHDInsightClusterSize -ClusterName <Cluster Name> -TargetInstanceCount <NewSize>


## <a name="grantrevoke-access"></a>Hozzáférés biztosítása/visszavonása
A HDInsight-fürtök a következő HTTP webszolgáltatásokat (ezen szolgáltatások mindegyikéhez rendelkezik RESTful végpontok) rendelkezik:

* ODBC
* JDBC
* Ambari
* Oozie
* Lépni a Templeton

Alapértelmezés szerint ezek a szolgáltatások hozzáférés vonatkozóan biztosított. Meg is visszavonási/engedélyezze a hozzáférést. Visszavonni:

    Revoke-AzureRmHDInsightHttpServicesAccess -ClusterName <Cluster Name>

Megadását:

    $clusterName = "<HDInsight Cluster Name>"

    # Credential option 1
    $hadoopUserName = "admin"
    $hadoopUserPassword = "<Enter the Password>"
    $hadoopUserPW = ConvertTo-SecureString -String $hadoopUserPassword -AsPlainText -Force
    $credential = New-Object System.Management.Automation.PSCredential($hadoopUserName,$hadoopUserPW)

    # Credential option 2
    #$credential = Get-Credential -Message "Enter the HTTP username and password:" -UserName "admin"

    Grant-AzureRmHDInsightHttpServicesAccess -ClusterName $clusterName -HttpCredential $credential

> [!NOTE]
> A hozzáférés biztosítása/visszavonása, visszaállítja, a fürt felhasználónevet és jelszót.
>
>

Ez a portálon keresztül is elvégezhető. Lásd: [felügyelheti a HDInsight az Azure portál használatával][hdinsight-admin-portal].

## <a name="update-http-user-credentials"></a>HTTP-felhasználó hitelesítő adatainak frissítése
Ugyanezt az eljárást, mint a [Grant/revoke HTTP access](#grant/revoke-access). Ha a fürt megadták a HTTP-hozzáférést, meg kell először vonja vissza.  És adja meg a hozzáférés új HTTP felhasználói hitelesítő adatokkal.

## <a name="find-the-default-storage-account"></a>Az alapértelmezett tárfiók keresése
A következő Powershell-parancsfájl bemutatja, hogyan kell az alapértelmezett tárfiók neve és a alapértelmezett tárfiók hívóbetűjét fürt.

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup
    $defaultStorageAccountName = ($cluster.DefaultStorageAccount).Replace(".blob.core.windows.net", "")
    $defaultBlobContainerName = $cluster.DefaultStorageContainer
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey

## <a name="find-the-resource-group"></a>Az erőforráscsoport keresése
Az erőforrás-kezelő módban egy Azure-erőforráscsoportot minden HDInsight-fürthöz tartozik.  Az erőforráscsoport megkeresése:

    $clusterName = "<HDInsight Cluster Name>"

    $cluster = Get-AzureRmHDInsightCluster -ClusterName $clusterName
    $resourceGroupName = $cluster.ResourceGroup


## <a name="submit-jobs"></a>Feladatok elküldéséhez
**Elküldeni a MapReduce-feladatok**

Lásd: [Windows-alapú hdinsight Hadoop-MapReduce futtatása minták](hdinsight-run-samples.md).

**Elküldeni a Hive-feladatok**

Lásd: [PowerShell használatával futtassa Hive lekérdezések](hdinsight-hadoop-use-hive-powershell.md).

**Elküldeni a Pig-feladatokhoz**

Lásd: [PowerShell használatával futtassa Pig-feladatokhoz](hdinsight-hadoop-use-pig-powershell.md).

**Sqoop feladatok küldéséhez**

Lásd: [Use Sqoop with HDInsight](hdinsight-use-sqoop.md).

**Oozie feladatok küldéséhez**

Lásd: [hadooppal megadásához és a munkafolyamat futtatása hdinsight használata Oozie](hdinsight-use-oozie.md).

## <a name="upload-data-to-azure-blob-storage"></a>Adatok feltöltése az Azure Blob storage
Lásd: [Adatok feltöltése a HDInsightba][hdinsight-upload-data].

## <a name="see-also"></a>Lásd még:
* [HDInsight parancsmag-referencia dokumentáció][hdinsight-powershell-reference]
* [HDInsight felügyelete az Azure-portál használatával][hdinsight-admin-portal]
* [HDInsight a parancssori felület felügyelete][hdinsight-admin-cli]
* [A HDInsight-fürtök létrehozása][hdinsight-provision]
* [Adatok feltöltése a HDInsightba][hdinsight-upload-data]
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
