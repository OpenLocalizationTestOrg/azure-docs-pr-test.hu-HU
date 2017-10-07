---
title: "a távoli asztal a HDInsight - Azure Hadoop Pig aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Pig parancs toorun Pig Latin nyilatkozatai egy távoli asztali kapcsolat tooa Windows-alapú Hadoop-fürt hdinsightban."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: e034a286-de0f-465f-8bf1-3d085ca6abed
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 2a4565fa827cd45fdbe6194b0486df93a6561084
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-from-a-remote-desktop-connection"></a>A távoli asztali kapcsolat-ről futtatva a Pig-feladatokhoz
[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Ez a dokumentum egy forgatókönyv biztosít hello Pig parancs toorun Pig Latin utasítások fürtről a távoli asztali kapcsolat tooa Windows-alapú HDInsight segítségével. A Pig Latin lehetővé teszi azok adatátalakítást, toocreate MapReduce alkalmazások helyett képezi le, és csökkentheti a funkciók.

> [!IMPORTANT]
> A távoli asztal a Windows hello operációs rendszert használó HDInsight-fürtök csak érhető el. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4-es vagy nagyobb, lásd: [HDInsight és az SSH használata a Pig](hdinsight-hadoop-use-pig-ssh.md) információ interaktív Pig közvetlenül egy feladat hello fürt parancsot a parancssorból.

## <a id="prereq"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, szüksége lesz a hello következő.

* (A HDInsight Hadoop) a Windows-alapú HDInsight-fürt
* Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

## <a id="connect"></a>Csatlakozzon a távoli asztal
A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="pig"></a>Hello Pig parancs használata
1. Miután egy távoli asztali kapcsolat, indítsa el a hello **Hadoop parancssori** hello ikonnal hello asztalon.
2. A következő toostart hello Pig parancs hello használata:

        %pig_home%\bin\pig

    Meg fog jelenni a `grunt>` kérdés.
3. Adja meg a következő utasítás hello:

        LOGS = LOAD 'wasb:///example/data/sample.log';

    Ez a parancs hello hello sample.log fájl tartalmát betölti a hello naplók fájlt. Hello fájl tartalmának hello hello a következő parancs használatával tekintheti meg:

        DUMP LOGS;
4. Hello adatok átalakítása minden rekordból egy reguláris kifejezést tooextract csak hello naplózási szintek alkalmazásával:

        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;

    Használhat **DUMP** tooview hello adatok hello átalakítás után. Ebben az esetben `DUMP LEVELS;`.
5. További, átalakítások alkalmazása a következő utasítások hello segítségével. Használjon `DUMP` tooview hello eredménye hello átalakítása lépések után.

    <table>
    <tr>
    <th>Utasítás</th><th>Funkció</th>
    </tr>
    <tr>
    <td>FILTEREDLEVELS = által LOGLEVEL SZINTJEINEK értéke nem null;</td><td>Hello naplózási szint null értéket tartalmazó sorok eltávolítása, és a FILTEREDLEVELS hello eredmények tárolja.</td>
    </tr>
    <tr>
    <td>GROUPEDLEVELS = csoport FILTEREDLEVELS által LOGLEVEL;</td><td>Csoportok hello sort a naplózási szint szerint, és tárolja a hello eredmények GROUPEDLEVELS be.</td>
    </tr>
    <tr>
    <td>GYAKORISÁGOT = foreach GROUPEDLEVELS csoport létrehozás módja LOGLEVEL, COUNT (FILTEREDLEVELS. LOGLEVEL), COUNT;</td><td>Egy új készletet hoz létre, amely tartalmazza az egyes egyedi napló adatok értéke szinten, és hány alkalommal következik be. A GYAKORISÁGOT történő tárolás</td>
    </tr>
    <tr>
    <td>EREDMÉNY = rendelés GYAKORISÁGOT által száma desc;</td><td>Az eredmény hello naplózási szintek száma (csökkenő), és tárolja rendeléseket</td>
    </tr>
    </table>
6.Az átalakítás hello eredménye hello segítségével is mentheti `STORE` utasítást. Például a következő parancs hello menti hello `RESULT` toohello **/example/data/pigout** könyvtárban lévő hello alapértelmezett tároló a fürt:

        STORE RESULT into 'wasb:///example/data/pigout'

   > [!NOTE]
   > hello adatok hello megadott könyvtár nevű fájlban tárolódnak **rész-nnnnn**. Ha hello könyvtár már létezik, kapni fog egy hibaüzenetet.
   >
   >
7. tooexit hello grunt parancssorból, írja be a következő utasítás hello.

        QUIT;

### <a name="pig-latin-batch-files"></a>A Pig Latin parancsfájlokat
Hello Pig parancs toorun Pig Latin található egy fájlban is használható.

1. Hello grunt kérdés kilépéskor nyissa meg a **Jegyzettömb** , és hozzon létre egy új fájlt **pigbatch.pig** a hello **PIG_HOME %** könyvtár.
2. Típus vagy Beillesztés hello következő sorok be hello **pigbatch.pig** fájlt, és mentse azt:

        LOGS = LOAD 'wasb:///example/data/sample.log';
        LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
        FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
        GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
        FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
        RESULT = order FREQUENCIES by COUNT desc;
        DUMP RESULT;
3. A következő toorun hello használata hello **pigbatch.pig** fájl hello pig parancs használatával.

        pig %PIG_HOME%\pigbatch.pig

    Hello kötegelt befejezését követően megjelenik kimenete, amely kell kell hello ugyanaz, mint ha használja a következő hello `DUMP RESULT;` hello előző lépésekben:

        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <a id="summary"></a>Summary (Összefoglalás)
Ahogy látja, a Pig parancs hello lehetővé teszi toointeractively MapReduce műveletek futtatása, vagy egy parancsfájlba tárolt Pig Latin feladatok futtatása.

## <a id="nextsteps"></a>Következő lépések
Általános információk a hdinsight Pig:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)
