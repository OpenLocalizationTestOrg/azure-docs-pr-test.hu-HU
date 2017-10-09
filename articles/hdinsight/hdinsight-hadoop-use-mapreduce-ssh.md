---
title: "aaaMapReduce és a hadooppal a Hdinsightban - Azure SSH-kapcsolat |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse SSH toorun MapReduce feladatokat, a HDInsight Hadoop használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 844678ba-1e1f-4fda-b9ef-34df4035d547
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.openlocfilehash: 9626577687fc5cc119a39d65a9c45298f57f81c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-with-hadoop-on-hdinsight-with-ssh"></a>Az SSH hdinsight Hadoop MapReduce használata

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ismerje meg, hogyan toosubmit MapReduce egy Secure Shell (SSH) kapcsolat tooHDInsight a feladatokat.

> [!NOTE]
> Ha már ismeri a Linux-alapú Hadoop használatával kiszolgálók, de új tooHDInsight, lásd: [Linux-alapú HDInsight tippek](hdinsight-hadoop-linux-information.md).

## <a id="prereq"></a>Előfeltételek

* (A HDInsight Hadoop) a Linux-alapú HDInsight-fürt

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy SSH-ügyfél. További információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md)

## <a id="ssh"></a>Csatlakozzon SSH

Csatlakozzon az SSH használatával toohello fürt. Például a következő parancs hello csatlakozik nevű tooa fürt **myhdinsight**:

```bash
ssh admin@myhdinsight-ssh.azurehdinsight.net
```

**Ha egy tanúsítvány-kulcsot használ SSH hitelesítés**, szükség lehet hello titkos kulcs toospecify hello helyét az ügyfél rendszerén, például:

```bash
ssh -i ~/mykey.key admin@myhdinsight-ssh.azurehdinsight.net
```

**Ha jelszót használhat SSH hitelesítés**, tooprovide hello jelszót van szüksége.

Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="hadoop"></a>Hadoop-parancsok használata

1. Után csatlakoztatott toohello HDInsight-fürtre, használja a következő parancs toostart MapReduce feladatot hello:

    ```bash
    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar wordcount /example/data/gutenberg/davinci.txt /example/data/WordCountOutput
    ```

    A paranccsal elindítja hello `wordcount` osztályt, amelyet hello `hadoop-mapreduce-examples.jar` fájlt. Hello használ `/example/data/gutenberg/davinci.txt` bemeneti és kimeneti dokumentumot tárolódik `/example/data/WordCountOutput`.

    > [!NOTE]
    > A MapReduce feladatot és hello példa adatokkal kapcsolatos további információkért lásd: [használata MapReduce a Hadoop on HDInsight](hdinsight-use-mapreduce.md).

2. hello feladat részletei bocsát ki, akkor feldolgozza, és információt hasonló toohello hello feladat befejezésekor a következő szöveget adja vissza:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623

3. Hello feladat befejezése után használja a következő parancs toolist hello kimeneti fájlok hello:

    ```bash
    hdfs dfs -ls /example/data/WordCountOutput
    ```

    Ez a parancs két fájlt megjelenítése `_SUCCESS` és `part-r-00000`. Hello `part-r-00000` fájl tartalmazza a feladat hello kimenet.

    > [!NOTE]
    > Bizonyos MapReduce-feladatok hello eredmények lehet, hogy e osztani több **rész-r-###** fájlokat. Ha igen, használjon hello ### utótag hello fájlok tooindicate hello sorrendjét.

4. tooview hello kimeneti, használja a következő parancs hello:

    ```bash
    hdfs dfs -cat /example/data/WordCountOutput/part-r-00000
    ```

    Ez a parancs szereplő hello szavak listáját jeleníti meg hello **wasb://example/data/gutenberg/davinci.txt** fájl- és hello száma minden szó történt. hello következő szövege hello tárolt adatokat használó hello fájlban egy példát:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Summary (Összefoglalás)

Ahogy látja, Hadoop parancsok egy egyszerűen toorun MapReduce-feladatok egy HDInsight-fürtöt, és a nézet hello feladatkiemenetét adja meg.

## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
