---
title: "aaaCreate Hadoop-fürtök PowerShell - Azure HDInsight használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Hadoop, HBase, Storm vagy Spark-fürtök Linux rendszeren a HDInsight Azure PowerShell használatával."
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
ms.openlocfilehash: 53afe4702f6b61a0720ceda48a4a34d7fa8797d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-linux-based-clusters-in-hdinsight-using-azure-powershell"></a>Linux-alapú fürtök létrehozása a Hdinsightban Azure PowerShell használatával

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Az Azure PowerShell egy hatékony parancsfájl-kezelési környezet, hogy toocontrol használ, és hello telepítése és kezelése a Microsoft Azure-ban a feladatok automatizálásához. Ez a dokumentum bemutatja, hogyan toocreate egy Linux-alapú HDInsight fürt Azure PowerShell használatával. Példa parancsfájl is tartalmaz.

> [!NOTE]
> Az Azure PowerShell a Windows-ügyfelek csak érhető el. Ha a Linux, Unix vagy Mac OS X-ügyfelet kell használnia, lásd: [hozzon létre egy Linux-alapú HDInsight-fürt Azure parancssori felület használatával](hdinsight-hadoop-create-linux-clusters-azure-cli.md) hello Azure CLI toocreate fürt használatáról.

## <a name="prerequisites"></a>Előfeltételek
Az eljárás megkezdése előtt hello következő kell rendelkeznie:

* Azure-előfizetés. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* [Azure PowerShell](/powershell/azure/install-azurerm-ps)

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnt. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Kövesse a lépéseket hello [Azure PowerShell telepítése](https://docs.microsoft.com/powershell/azure/install-azurerm-ps) tooinstall hello legújabb verziót. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.

## <a name="create-cluster"></a>Fürt létrehozása

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

HDInsight-fürtök Azure PowerShell használatával toocreate, végre kell hajtania a következő eljárások hello:

* Egy Azure erőforráscsoport létrehozása
* Azure Storage-fiók létrehozása
* Egy Azure Blob-tároló létrehozása
* HDInsight-fürtök létrehozása

hello alábbi parancsfájl bemutatja, hogyan toocreate új fürt:

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster.ps1?range=5-71)]

hello hello fürt bejelentkezési azonosítóhoz megadott használt toocreate hello hello fürt Hadoop-felhasználói fiókot. Használja a fiók tooconnect tooservices, például a web UI vagy a REST API-k hello fürtön található.

hello hello SSH-felhasználó számára megadott értékei használt toocreate hello SSH-felhasználó hello fürt. Ezzel a távoli SSH-munkamenet toostart fiókot hello fürtön, és a feladatok futtatásához. További információkért lásd: hello [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md) dokumentum.

> [!IMPORTANT]
> Ha azt tervezi, toouse több mint 32 munkavégző csomópontokhoz (vagy a fürt létrehozásakor vagy méretezési hello a fürt létrehozása után), is meg kell egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM-mal rendelkező.
>
> A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

Is eltarthat, too20 perc toocreate egy fürt.

## <a name="create-cluster-configuration-object"></a>Fürt létrehozása: konfigurációs objektum

Egy HDInsight konfigurációs objektum használatával is létrehozhat `New-AzureRmHDInsightClusterConfig` parancsmag. Ezt követően módosíthatja a konfigurációs objektum tooenable további konfigurációs lehetőségek a fürt számára. Végül, használja a hello `-Config` hello paramétere `New-AzureRmHDInsightCluster` parancsmag toouse hello konfigurációs.

hello következő parancsfájlt hoz létre egy konfigurációs objektum tooconfigure egyik R Server a HDInsight-fürt típusa. hello konfiguráció lehetővé teszi, hogy egy élcsomópontot Rstudióból és egy további storage-fiókot.

[!code-powershell[main](../../powershell_scripts/hdinsight/create-cluster/create-cluster-with-config.ps1?range=59-98)]

> [!WARNING]
> A storage-fiók egy másik helyen található mint hello HDInsight-fürt használata nem támogatott. Ez a példa használatakor hello további storage-fiók létrehozása a hello hello kiszolgáló ugyanazon a helyen.

## <a name="customize-clusters"></a>Fürtök személyre szabása

* Lásd: [testreszabása HDInsight-fürtök használata rendszerindítási](hdinsight-hadoop-customize-cluster-bootstrap.md#use-azure-powershell).
* Lásd: [testreszabása HDInsight-fürtök használata parancsfájlművelet](hdinsight-hadoop-customize-cluster-linux.md).

## <a name="delete-hello-cluster"></a>Hello fürt törlése

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight-fürtre, használja a következő erőforrások toolearn hogyan hello toowork a fürthöz.

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
* [Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: A Spark on HDInsight használata valós idejű streamelési alkalmazások összeállítására](hdinsight-apache-spark-eventhub-streaming.md)

