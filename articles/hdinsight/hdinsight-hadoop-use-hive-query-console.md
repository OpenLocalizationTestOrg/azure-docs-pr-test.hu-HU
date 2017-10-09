---
title: "a hdinsight - Azure lekérdezés konzol hello Hadoop Hive aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello webalapú konzol lekérdezés toorun Hive-lekérdezések egy HDInsight hadoop cluster a böngészőből."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5ae074b0-f55e-472d-94a7-005b0e79f779
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 621882082c9a07655d34b8dc980b8e47dd04b745
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-query-console"></a>Hello lekérdezés konzol használatával Hive-lekérdezések futtatása
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Ebből a cikkből megtudhatja, hogyan toouse hello HDInsight lekérdezés konzol toorun Hive-lekérdezések egy HDInsight hadoop cluster a böngészőből.

> [!IMPORTANT]
> hello HDInsight lekérdezés konzol a Windows-alapú HDInsight-fürtök csak érhető el. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> HDInsight 3.4-es vagy nagyobb, lásd: [lekérdezések futtatása Hive Ambari Hive nézetben](hdinsight-hadoop-use-hive-ambari-view.md) információ Hive-lekérdezések egy webböngészőből.

## <a id="prereq"></a>Előfeltételek
toocomplete hello cikkben leírt lépéseket, szüksége lesz a hello következő.

* A Windows-alapú HDInsight Hadoop-fürt
* Modern webböngésző

## <a id="run"></a>Hello lekérdezés konzol használatával Hive-lekérdezések futtatása
1. Nyisson meg egy webböngészőt, és keresse meg a túl**https://CLUSTERNAME.azurehdinsight.net**, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz. Ha a rendszer kéri, adja meg a hello felhasználónevet és a hello fürt létrehozásakor használt jelszót.
2. Hello hivatkozások hello oldal hello tetején, válassza ki **Hive szerkesztő**. Lehet, hogy szeretné-e a HDInsight-fürt hello toorun használt tooenter hello HiveQL utasítások űrlap megjelenik.

    ![hello hive szerkesztő](./media/hdinsight-hadoop-use-hive-query-console/queryconsole.png)

    Cserélje le a hello szöveg `Select * from hivesampletable` a következő HiveQL utasítások hello:

        set hive.execution.engine=tez;
        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * **DROP TABLE**: hello tábla és hello adatfájl törlése, ha hello tábla már létezik.
   * **A külső tábla létrehozása**: táblát hoz létre egy új "külső" struktúra. Külső táblák csak hello tábladefiníció tárolása Hive; hello adatok hello eredeti helyen marad.

     > [!NOTE]
     > Külső táblák kell használni, amikor hello alapjául szolgáló adatok toobe frissíteni az külső forrásból (például egy automatizált adatok feltöltési folyamat) vagy egy másik MapReduce művelet által várt, de azt szeretné, hogy Hive toouse hello legfrissebb adatokat lekérdezi.
     >
     > A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.
     >
     >
   * **SOR formátum**: közli Hive hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.
   * **AS TEXTFILE helyen tárolt**: közli Hive hello adatok esetén tárolt (hello példaadatokat/könyvtár), és szövegként tárolt
   * **Válassza ki**: válassza ki az összes sorok számát ahol oszlop **t4** tartalmaz hello értéket **[hiba]**. Ez visszaadja-e érték **3** mert három ezt az értéket tartalmazó sorok.
   * **INPUT__FILE__NAME PÉLDÁUL "%.log"** -struktúra azt ismerteti, amely jelenleg csak a befejezési fájlok kell vissza adatokat. napló. Ez korlátozza a hello keresési toohello sample.log fájlt, amely hello adatokat tartalmaz, és a nem ad vissza adatot más példa, amely nem egyezik a meghatározott hello séma adatfájlok tartja azt.
3. Kattintson a **nyújt**. Hello **feladat munkamenet** hello: hello lap alján megjelenjen-e meg hello feladat részleteit.
4. Ha hello **állapot** mező túl megváltozik**befejezve**, jelölje be **részleteinek megtekintése** hello feladat. Hello Részletek lapján hello **Job Output** tartalmaz `[ERROR]    3`. Használhatja a hello **letöltése** hello feladat eredményének hello tartalmazó fájl alapján ez a mező toodownload gombra.

## <a id="summary"></a>Summary (Összefoglalás)
Ahogy látja, lekérdezés konzol segítségével egy egyszerű módot toorun hello Hive lekérdezi a HDInsight-fürtöt, hello feladat állapotának figyelése és hello kimeneti beolvasása.

Hive lekérdezés konzol toorun Hive-feladatok használatáról további toolearn válasszon **bevezetés** tetején hello hello lekérdezés konzolt, majd használja az hello minták által biztosított. Minden minta bemutatja, hogyan hello folyamatot, amely Hive tooanalyze adatok, beleértve a magyarázatot hello HiveQL utasításokról hello mintában használt használatával.

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



[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
