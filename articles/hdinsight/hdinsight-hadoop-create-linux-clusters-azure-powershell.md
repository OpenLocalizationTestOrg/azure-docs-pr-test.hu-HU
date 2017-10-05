---
title: "PowerShell - Azure HDInsight Hadoop-fürtök létrehozása |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre Hadoop, HBase, Storm vagy Spark-fürtök Linux rendszeren működő HDInsight-hoz Azure PowerShell használatával."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 4208deca-d64a-45e1-8948-2673d5d7678c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/10/2017
ms.author: nitinme
ms.openlocfilehash: ca75974e6ec4f60739137d4cb5458bbfd735de3e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Linux-alapú fürtök létrehozása a Hdinsightban Azure PowerShell használatával

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, melyekkel automatizálhatja a központi telepítési és felügyeleti a alkalmazások a Microsoft Azure-ban, és szabályozhatja. Ez a dokumentum tájékoztatást ad azokról a Linux-alapú HDInsight-fürt létrehozása az Azure PowerShell használatával. Példa parancsfájl is tartalmaz.

> [!NOTE]
> Az Azure PowerShell a Windows-ügyfelek csak érhető el. Ha a Linux, Unix vagy Mac OS X-ügyfelet kell használnia, lásd: [hozzon létre egy Linux-alapú HDInsight-fürt Azure parancssori felület használatával](hdinsight-hadoop-create-linux-clusters-azure-cli.md) fürt létrehozása az Azure parancssori felület használatával kapcsolatos információkat.

## <a name="prerequisites"></a>Előfeltételek
Az eljárás megkezdése előtt a következőket kell tartalmaznia:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. A jelen dokumentumban leírt lépések az új HDInsight-parancsmagokat használják, amelyek az Azure Resource Managerrel működnek.
    >
    > Kövesse a lépéseket a [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) Azure PowerShell legújabb verziójának telepítéséhez. Ha vannak olyan parancsprogramjai, amelyeket módosítani kell az új, az Azure Resource Managerrel működő parancsmagok használatához, tekintse meg az alábbi cikket: [Migrating to Azure Resource Manager-based development tools for HDInsight clusters](hdinsight-hadoop-development-using-azure-resource-manager.md) (Az Azure Resource Manager-alapú fejlesztési eszközökre való áttérés HDInsight-fürtök esetén).

## <a name="create-cluster"></a>Fürt létrehozása

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight-fürtök létrehozása az Azure PowerShell használatával, a következő eljárásokat kell végrehajtania:

* Egy Azure erőforráscsoport létrehozása
* Azure Storage-fiók létrehozása
* Egy Azure Blob-tároló létrehozása
* HDInsight-fürtök létrehozása

Az alábbi parancsfájl bemutatja, hogyan hozhat létre egy új fürtöt:

[!code-powershell[fő](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

A fürt bejelentkezési azonosítóhoz megadott értékek a Hadoop felhasználói fiók a fürt létrehozásához használt. Ez a fiók használatával csatlakozhat például a web UI vagy a REST API-k a fürtön tárolt szolgáltatások.

Az SSH-felhasználó számára megadott érték az SSH-felhasználó a fürt létrehozásához használt. Ez a fiók használatával indítson el egy távoli SSH-munkamenetet a fürtön, és a feladatok futtatásához. További információ: [SSH használata a HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

> [!IMPORTANT]
> Ha több mint 32 munkavégző csomópontokhoz (vagy a fürt létrehozásakor, vagy a fürt létrehozása után skálázással) használja, is meg kell egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal rendelkező.
>
> A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

A fürt létrehozásához akár 20 percig is tarthat.

## <a name="create-cluster-configuration-object"></a>Fürt létrehozása: konfigurációs objektum

Egy HDInsight konfigurációs objektum használatával is létrehozhat `New-AzureRmHDInsightClusterConfig` parancsmag. Ezt követően módosíthatja a konfigurációs objektum ahhoz, hogy a fürt további konfigurációs beállítások. Végül a `-Config` paramétere a `New-AzureRmHDInsightCluster` parancsmag segítségével használja a konfigurációt.

A következő parancsfájl egy konfigurációs objektumot az R-kiszolgáló konfigurálása a HDInsight-fürt típusa hoz létre. A konfiguráció lehetővé teszi, hogy egy élcsomópontot Rstudióból és egy további storage-fiókot.

[!code-powershell[fő](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> A storage-fiók egy másik helyen, mint a HDInsight-fürt használata nem támogatott. Ha ebben a példában, és a kiszolgáló ugyanazon a helyen hozza létre a további tárhely fiókot.

## <a name="customize-clusters"></a>Fürtök személyre szabása

* Lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-the-cluster"></a>A fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozta a HDInsight-fürtöt, a következő források segítségével megtudhatja, hogyan működnek a fürthöz.

### <a name="hadoop-clusters"></a>Hadoop-fürtök

* [A Hive használata a HDInsightban](hdinsight-use-hive.md)
* [A Pig használata a HDInsightban](hdinsight-use-pig.md)
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)

### <a name="hbase-clusters"></a>HBase-fürtökkel

* [Az a HDInsight HBase első lépései](hdinsight-hbase-tutorial-get-started-linux.md)
* [A HDInsight HBase Java-alkalmazások fejlesztése](hdinsight-hbase-build-java-maven-linux.md)

### <a name="storm-clusters"></a>Storm-fürtök

* [Java-topológiák fejlesztése hdinsight alatt futó Storm](hdinsight-storm-develop-java-topology.md)
* [A HDInsight alatt futó Storm Python-összetevőket használnak](hdinsight-storm-develop-python-topology.md)
* [Telepítheti és figyelheti a HDInsight alatt futó Storm topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)

### <a name="spark-clusters"></a>A Spark-fürtök

* [Önálló alkalmazás létrehozása a Scala használatával](hdinsight-apache-spark-create-standalone-application.md)
* [Feladatok távoli futtatása Spark-fürtön a Livy használatával](hdinsight-apache-spark-livy-rest-interface.md)
* [Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel](hdinsight-apache-spark-use-bi-tools.md)
* [Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)

