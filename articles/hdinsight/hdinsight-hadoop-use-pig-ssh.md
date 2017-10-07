---
title: "a HDInsight-fürtök - Azure Hadoop Pig with SSH aaaUse |} Microsoft Docs"
description: "Megtudhatja, hogyan csatlakozzon SSH tooa Linux-alapú Hadoop-fürtöt, és majd használata hello Pig parancs toorun Pig Latin utasítások interaktív módon, vagy egy kötegelt feladat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b646a93b-4c51-4ba4-84da-3275d9124ebe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.openlocfilehash: 1da303e239b537e6b331b1d33010058582718c90
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-on-a-linux-based-cluster-with-hello-pig-command-ssh"></a>Futtassa a Pig-feladatokhoz hello Pig parancs (SSH) rendelkező Linux-alapú fürtön

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ismerje meg, hogyan toointeractively futtassa egy SSH-kapcsolat tooyour HDInsight-fürt a Pig-feladatokhoz. hello Pig Latin programozási nyelv, amelyeket alkalmazott toohello bemeneti adatok tooproduce szükséges hello toodescribe átalakítások lehetővé teszi.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések szükséges egy Linux-alapú HDInsight-fürtöt. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="ssh"></a>Csatlakozzon SSH

SSH tooconnect tooyour HDInsight-fürt használatára. hello alábbi példa csatlakozik nevű tooa fürt **myhdinsight** nevű hello fiókként **sshuser**:

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net

**Ha megadott egy SSH-hitelesítési tanúsítvány kulcs** hello HDInsight-fürt létrehozása után, szükség lehet hello titkos kulcs toospecify hello helyét az ügyfélrendszeren.

    ssh sshuser@myhdinsight-ssh.azurehdinsight.net -i ~/mykey.key

**Ha a megadott jelszó SSH hitelesítés** hello HDInsight-fürt létrehozásakor adja meg a hello jelszót.

Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="pig"></a>Hello Pig parancs használata

1. A csatlakozás után a következő parancs hello használatával indítsa el az hello Pig parancssori felület (CLI):

        pig

    Néhány percet, megtekintheti a `grunt>` kérdés.

2. Adja meg a következő utasítás hello:

        LOGS = LOAD '/example/data/sample.log';

    Ez a parancs hello hello sample.log fájl tartalmát betölti a NAPLÓKAT. Hello hello fájl tartalmát a következő utasítás hello segítségével tekintheti meg:

        DUMP LOGS;

3. A következő átalakító hello adatokat úgy, hogy egy reguláris kifejezést tooextract csak hello naplózási szint alkalmazza minden rekordból hello utasítás a következő használatával:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Használhat **DUMP** tooview hello adatok hello átalakítás után. Ebben az esetben használjon `DUMP LEVELS;`.

4. További, átalakítások alkalmazása a következő táblázat hello hello utasítások segítségével:

    | A Pig Latin utasítás | Milyen hello utasítás does |
    | ---- | ---- |
    | `FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;` | Hello naplózási szint null értéket tartalmazó sorok eltávolítása, és tárolja a hello eredmények `FILTEREDLEVELS`. |
    | `GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;` | Csoportok hello sort a naplózási szint szerint, és tárolja a hello eredmények `GROUPEDLEVELS`. |
    | `FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;` | Hoz létre, amely tartalmazza az egyes egyedi napló adatok értéke szinten, és hány alkalommal következik be. hello adatkészlet tárolja azokat `FREQUENCIES`. |
    | `RESULT = order FREQUENCIES by COUNT desc;` | Rendelések hello naplózási szintek száma (csökkenő) szerint, és tárolja a `RESULT`. |

    > [!TIP]
    > Használjon `DUMP` tooview hello eredménye hello átalakítása lépések után.

5. Az átalakítás hello eredménye hello segítségével is mentheti `STORE` utasítást. Például a következő utasítás hello menti hello `RESULT` toohello `/example/data/pigout` a fürt hello alapértelmezett tároló könyvtár:

        STORE RESULT into '/example/data/pigout';

   > [!NOTE]
   > hello adatok hello megadott könyvtár nevű fájlban tárolódnak `part-nnnnn`. Ha hello könyvtár már létezik, akkor egy hibaüzenet.

6. tooexit hello grunt parancssorból, írja be a következő utasítás hello:

        QUIT;

### <a name="pig-latin-batch-files"></a>A Pig Latin parancsfájlokat

Hello Pig parancs toorun fájlban található Pig Latin is használható.

1. Hello grunt kérdés kilépéskor használata hello következő parancsot a toopipe STDIN fájlba `pigbatch.pig`. A fájl hello kezdőkönyvtárban hello SSH felhasználói fiók jön létre.

        cat > ~/pigbatch.pig

2. Írja be vagy illessze be az alábbi hello, és használja a Ctrl + D, amikor befejeződött az.

        LOGS = LOAD '/example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;

3. Használjon hello következő parancsot a toorun hello `pigbatch.pig` fájl hello Pig parancs használatával.

        pig ~/pigbatch.pig

    Miután hello kötegelt feladat befejeződik, a következő kimeneti hello jelenik meg:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)


## <a id="nextsteps"></a>Következő lépések

Általános információk a hdinsight a Pig tekintse meg a következő dokumentum hello:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

A HDInsight hadooppal a más módon toowork további információkért lásd: a következő dokumentumok hello:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
