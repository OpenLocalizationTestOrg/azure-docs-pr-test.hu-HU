---
title: "aaaUse Hadoop Hive és a távoli asztal a HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconnect tooHadoop fürt a Hdinsightban távoli asztal használatával, és futtassa a Hive-lekérdezések hello Hive parancssori felület használatával."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 8c228e35-d58a-4f22-917a-1d20c9da89b4
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: f86ffc1be33a8b0b2346d1a1388e5dfa6d0f8777
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-hadoop-on-hdinsight-with-remote-desktop"></a>A Hive használata a HDInsight a távoli asztal hadooppal
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebből a cikkből megtudhatja, hogyan tooconnect tooan HDInsight fürt távoli asztal használatával, és futtassa a Hive lekérdezi hello Hive parancssori felület (CLI) használatával.

> [!IMPORTANT]
> A távoli asztal a Windows hello operációs rendszert használó HDInsight-fürtök csak érhető el. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4-es vagy nagyobb, lásd: [használata a HDInsight és Beeline Hive](hdinsight-hadoop-use-hive-beeline.md) információk Hive-lekérdezéseket közvetlenül hello fürtben futó parancsot a parancssorból.

## <a id="prereq"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, szüksége lesz a következő hello:

* (A HDInsight Hadoop) a Windows-alapú HDInsight-fürt
* Windows 10, Windows 8 vagy Windows 7 rendszert futtató ügyfélszámítógép

## <a id="connect"></a>Csatlakozzon a távoli asztal
A távoli asztal engedélyezése a hello HDInsight-fürthöz, majd kösse a tooit: hello utasításokat követve [csatlakozás RDP tooHDInsight-fürtök](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

## <a id="hive"></a>Hello Hive parancs használata
Hello HDInsight-fürthöz toohello asztali csatlakozáskor használja a következő lépéseket toowork a Hive hello:

1. Hello HDInsight asztalról start hello **Hadoop parancssori**.
2. Adja meg a következő parancs toostart hello Hive CLI hello:

        %hive_home%\bin\hive

    Hello CLI indításakor megjelenik hello CLI struktúra kérdés: `hive>`.
3. Hello CLI, adja meg a következő utasítások toocreate nevű új tábla hello **log4jLogs** mintaadatok használatával:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * **DROP TABLE**: hello tábla és hello adatfájl törlése, ha hello tábla már létezik.
   * **A külső tábla létrehozása**: táblát hoz létre egy új "külső" struktúra. Külső táblák csak hello tábladefiníció struktúra (hello adatok marad az eredeti helyükre hello) tárolja.

     > [!NOTE]
     > Külső táblák kell használni, amikor hello alapjául szolgáló adatok toobe frissíteni az külső forrásból (például egy automatizált adatok feltöltési folyamat) vagy egy másik MapReduce művelet által várt, de azt szeretné, hogy Hive toouse hello legfrissebb adatokat lekérdezi.
     >
     > A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.
     >
     >
   * **SOR formátum**: közli Hive hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.
   * **AS TEXTFILE helyen tárolt**: közli Hive hello adatok esetén tárolt (hello példaadatokat/könyvtár), és szövegként tárolt.
   * **Válassza ki**: választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**. Ez visszaadja-e érték **3** mert három ezt az értéket tartalmazó sorok.
   * **INPUT__FILE__NAME PÉLDÁUL "%.log"** -struktúra azt ismerteti, amely jelenleg csak a befejezési fájlok kell vissza adatokat. napló. Ez korlátozza a hello keresési toohello sample.log fájlt, amely hello adatokat tartalmaz, és a nem ad vissza adatot más példa, amely nem egyezik a meghatározott hello séma adatfájlok tartja azt.
4. A következő utasítások toocreate nevű új "belső" tábla használata hello **errorLogs**:

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * **Hozzon létre Ha nem létezik táblázat**: egy táblát hoz létre, ha még nem létezik. Mivel hello **külső** kulcsszó nem használható, ez egy belső tábla, amely hello Hive-adatraktárban tárolja, és Hive teljesen kezeli.

     > [!NOTE]
     > Eltérően **külső** táblák, egy belső tábla is eldobása hello alapjául szolgáló adatok törlése.
     >
     >
   * **TÁROLT AS ORC**: hello eltárolja optimalizált sor oszlopos (ORC) formátumban. Ez az egy magas optimalizált és hatékony formátumú Hive adatainak tárolásához.
   * **ÍRJA FELÜL AZ INSERT... Válassza ki**: hello sorok kiválaszt **log4jLogs** tartalmazó tábla **[hiba]**, majd Beszúrások adatok hello be hello **errorLogs** tábla.

     tartalmaz, amely csak a sorok, amely tooverify **[hiba]** oszlop t4 voltak tárolt toohello **errorLogs** table, használja a következő utasítás tooreturn összes sorát hello hello **errorLogs**:

       Válassza ki * a errorLogs;

     Három sor adat vissza kell adni, az összes tartalmazó **[hiba]** az oszlop t4.

## <a id="summary"></a>Summary (Összefoglalás)
Ahogy látja, hello hello Hive parancs biztosít egy egyszerűen toointeractively futtathat Hive-lekérdezéseket a HDInsight-fürtöt, a figyelő hello feladat állapota, hello kimeneti beolvasása.

## <a id="nextsteps"></a>Következő lépések
Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Ha a Hive Tez használ, tekintse meg a hello dokumentumok hibakeresési információkat a következő:

* [A Windows-alapú HDInsight hello Tez felhasználói felület használata](hdinsight-debug-tez-ui.md)
* [A Linux-alapú HDInsight Ambari Tez nézet hello használata](hdinsight-debug-ambari-tez-view.md)

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/


[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md





[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
