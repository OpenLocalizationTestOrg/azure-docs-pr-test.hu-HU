---
title: "aaaMapReduce és a távoli asztal hadooppal a Hdinsightban - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse távoli asztal tooconnect tooHadoop HDInsight és futtatási MapReduce-feladatok."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 9d3a7b34-7def-4c2e-bb6c-52682d30dee8
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: bdbbcf59ca86dd1b170471aa9e12334a04c23667
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight-with-remote-desktop"></a>A távoli asztal hdinsight Hadoop MapReduce használata
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Ebből a cikkből megtudhatja, hogyan tooconnect tooa a HDInsight Hadoop cluster távoli asztal használatával, és futtassa a MapReduce-feladatok hello Hadoop parancs használatával.

> [!IMPORTANT]
> Távoli asztali Windows-alapú HDInsight-fürtök csak érhető el. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4-es vagy nagyobb, lásd: [az SSH használata MapReduce](hdinsight-hadoop-use-mapreduce-ssh.md) toohello HDInsight-fürthöz kapcsolódó, és a MapReduce-feladatok futtatása.

## <a id="prereq"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:

* (A HDInsight Hadoop) a Windows-alapú HDInsight-fürt
* Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

## <a id="connect"></a>Csatlakozzon a távoli asztal
A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hadoop"></a>Hello Hadoop parancs használata
Ha csatlakoztatott toohello asztali hello HDInsight-fürthöz, használja a hello lépéseket toorun MapReduce feladatot hello Hadoop parancs használatával a következő:

1. Hello HDInsight asztalról start hello **Hadoop parancssori**. Ekkor megnyílik egy új parancssort a hello **c:\apps\dist\hadoop-&lt;verziószám >** directory.

   > [!NOTE]
   > hello verziószám változik Hadoop frissül. Hello **HADOOP_HOME** környezeti változó használt toofind hello elérési út is lehet. Például `cd %HADOOP_HOME%` módosítások könyvtárak toohello Hadoop könyvtár, anélkül, hogy Ön tooknow hello verziószáma.
   >
   >
2. toouse hello **Hadoop** parancs egy példa MapReduce feladatot toorun, a következő parancs hello használata:

        hadoop jar hadoop-mapreduce-examples.jar wordcount wasb:///example/data/gutenberg/davinci.txt wasb:///example/data/WordCountOutput

    Ekkor elindul a hello **wordcount** osztályt, amelyet hello **hadoop-mapreduce-examples.jar** hello aktuális könyvtárban található fájl. Bemeneti, használja a hello **wasb://example/data/gutenberg/davinci.txt** dokumentum, és a kimeneti tárolódik: **wasb: / / / Példa/data/WordCountOutput**.

   > [!NOTE]
   > a MapReduce feladatot és hello példa adatokkal kapcsolatos további információkért lásd: <a href="hdinsight-use-mapreduce.md">MapReduce használata a HDInsight Hadoop a</a>.
   >
   >
3. hello feladat részletei bocsát ki, a feldolgozása, valamint hello feladatot ad vissza adatokat hasonló toohello következő:

        File Input Format Counters
        Bytes Read=1395666
        File Output Format Counters
        Bytes Written=337623
4. Ha hello folyamat befejeződik, használja a következő parancs toolist hello kimeneti fájl helye: hello **wasb://example/data/WordCountOutput**:

        hadoop fs -ls wasb:///example/data/WordCountOutput

    Ez megjelenjen-e két fájlt **_SUCCESS** és **rész-r-00000**. Hello **rész-r-00000** fájl tartalmazza a feladat hello kimenet.

   > [!NOTE]
   > Bizonyos MapReduce-feladatok hello eredmények lehet, hogy e osztani több **rész-r-###** fájlokat. Ha igen, használjon hello ### utótag hello fájlok tooindicate hello sorrendjét.
   >
   >
5. tooview hello kimeneti, használja a következő parancs hello:

        hadoop fs -cat wasb:///example/data/WordCountOutput/part-r-00000

    Ez hello szereplő hello szavak listáját jeleníti meg **wasb://example/data/gutenberg/davinci.txt** fájl hello száma minden szó történt. hello következő az hello fájlban szereplő adatok hello:

        wreathed        3
        wreathing       1
        wreaths         1
        wrecked         3
        wrenching       1
        wretched        6
        wriggling       1

## <a id="summary"></a>Summary (Összefoglalás)
Ahogy látja, hello Hadoop parancs Ez egy egyszerű módot toorun MapReduce-feladatok egy HDInsight-fürtöt, és a nézet hello feladatkiemenetét.

## <a id="nextsteps"></a>Következő lépések
Általános információk a hdinsight MapReduce-feladatok:

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
