---
title: "aaaCreate Hadoop-fürtök parancssori - hello Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate HDInsight-fürtök hello platformfüggetlen Azure CLI 1.0."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 50b01483-455c-4d87-b754-2229005a8ab9
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 5295b01054b8c23df0e3b75a3e0e8c933ac48b3c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hdinsight-clusters-using-hello-azure-cli"></a>Hello Azure CLI használata a HDInsight-fürtök létrehozása

[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

Ez a dokumentum az útmutató egy HDInsight 3.5 hello Azure CLI 1.0 fürtöt hoz létre hello szükséges lépések.

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).


## <a name="prerequisites"></a>Előfeltételek

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

* **Azure parancssori felület (CLI)**. hello jelen dokumentumban leírt lépések alapján utolsó történő teszteléskor az Azure CLI 0.10.14 verziója.

    > [!IMPORTANT]
    > Ebben a dokumentumban hello lépések nem használhatóak az Azure CLI 2.0. Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök létrehozása.

## <a name="log-in-tooyour-azure-subscription"></a>Jelentkezzen be tooyour Azure-előfizetés

Részletes ismertetését lásd: hello lépésekkel [tooan Azure-előfizetés csatlakoztatja a hello Azure parancssori felület (CLI)](../xplat-cli-connect.md) , és csatlakozzon a feliratkozás tooyour hello **bejelentkezési** metódust.

## <a name="create-a-cluster"></a>Fürt létrehozása

a parancssorból, például a PowerShell vagy a Bash hello a következő lépéseket kell végrehajtani.

1. A következő parancs tooauthenticate tooyour Azure-előfizetés hello használata:

        azure login

    Ön felszólító tooprovide vannak, a nevét és jelszavát. Ha több Azure-előfizetéssel rendelkezik, `azure account set <subscriptionname>` tooset hello-előfizetéssel, amely az Azure parancssori felület hello parancsok használata.

2. Váltás tooAzure Resource Manager módra a következő parancs hello használata:

        azure config mode arm

3. Hozzon létre egy erőforráscsoportot. Ez az erőforráscsoport hello HDInsight-fürtöt tartalmaz, és a kapcsolódó tárfiók.

        azure group create groupname location

    * Cserélje le `groupname` hello csoport egyedi névvel.

    * Cserélje le `location` a hello földrajzi régiót, amelyben a kívánt toocreate hello csoport.

       Az érvényese helyek listáját, használja a hello `azure location list` parancsot, és használja a hello hello helyek egyikén `Name` oszlop.

4. Hozzon létre egy tárfiókot. Ez a tárfiók hello alapértelmezett tárolóként szolgál hello HDInsight-fürthöz.

        azure storage account create -g groupname --sku-name RAGRS -l location --kind Storage storagename

    * Cserélje le `groupname` hello nevű hello előző lépésben létrehozott hello csoport.

    * Cserélje le `location` a hello hello előző lépésben használt ugyanazon a helyen.

    * Cserélje le `storagename` hello tárfiók egyedi névvel.

        > [!NOTE]
        > További információt ezen parancs hello paraméterekkel, `azure storage account create -h` tooview súgó ennél a parancsnál.

5. Kérje le hello tooaccess hello tárfiókot használja.

        azure storage account keys list -g groupname storagename

    * Cserélje le `groupname` a hello erőforráscsoport-név.
    * Cserélje le `storagename` hello nevű hello tárfiók.

     A visszaadott hello adatokat, mentse a hello `key` értékének `key1`.

6. HDInsight-fürtök létrehozása.

        azure hdinsight cluster create -g groupname -l location -y Linux --clusterType Hadoop --defaultStorageAccountName storagename.blob.core.windows.net --defaultStorageAccountKey storagekey --defaultStorageContainer clustername --workerNodeCount 3 --userName admin --password httppassword --sshUserName sshuser --sshPassword sshuserpassword clustername

    * Cserélje le `groupname` a hello erőforráscsoport-név.

    * Cserélje le `Hadoop` , hogy kívánja-e toocreate hello fürt típussal. Például `Hadoop`, `HBase`, `Kafka`, `Spark`, vagy `Storm`.

     > [!IMPORTANT]
     > HDInsight fürtök különböző típusainak, amely toohello munkaterhelés térjen vagy technológia, amely a fürt hello hangolt. Nincs nem támogatott metódus toocreate egy fürt, amely többféle, például a Storm és HBase egy fürtön egyesíti.

    * Cserélje le `location` az előző lépésben használt ugyanazon a helyen hello.

    * Cserélje le `storagename` a hello tárfiók neve.

    * Cserélje le `storagekey` hello előző lépésben beolvasott hello kulccsal.

    * A hello `--defaultStorageContainer` paraméter, használjon hello azonos nevét, ahogy hello fürt használja.

    * Cserélje le `admin` és `httppassword` hello névvel és jelszóval kívánja toouse hello fürt keresztül HTTPS elérésekor.

    * Cserélje le `sshuser` és `sshuserpassword` hello felhasználónévvel és jelszóval kívánja toouse hello fürt SSH használatával való hozzáféréskor

    > [!IMPORTANT]
    > Ebben a példában a fürt két munkavégző megjegyzéseket hoz létre. Skálázási műveletek elvégzésével fürt létrehozása után is módosíthatja hello feldolgozó csomópontok száma. Ha több mint 32 munkavégző csomópontokhoz használatát tervezi, majd ki kell választania egy átjárócsomóponttal mérete legalább 8 maggal és 14 GB RAM. Hello segítségével beállíthatja a hello átjárócsomópont mérete `--headNodeSize` paraméter fürt létrehozása során.
    >
    > A csomópont-méretek és a társuló költségeket további információkért lásd: [HDInsight árképzési](https://azure.microsoft.com/pricing/details/hdinsight/).

    Hello fürt létrehozási folyamat toofinish több percig is eltarthat. Általában körülbelül 15.

## <a name="troubleshoot"></a>Hibaelhárítás

Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).

## <a name="next-steps"></a>Következő lépések

Most, hogy sikeresen létrehozott egy HDInsight-fürtjéhez hello Azure CLI, használja a következő toolearn hogyan hello toowork a fürthöz:

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
