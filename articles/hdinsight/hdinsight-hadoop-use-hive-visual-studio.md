---
title: a Visual Studio - Azure HDInsight (Hadoop) a Data Lake tools aaaHive |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Data Lake tools az Apache Hive Visual Studio toorun az Azure HDInsight az Apache Hadoop-lekérdezések."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2b3e672a-1195-4fa5-afb7-b7b73937bfbe
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.openlocfilehash: dc76974c02cf68bcf701b2b155842c9e9c5cb988
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hello-data-lake-tools-for-visual-studio"></a>Futtathat Hive-lekérdezéseket hello Data Lake tools for Visual Studio használatával

Ismerje meg, hogyan toouse hello Data Lake Visual Studio tooquery Apache Hive eszközei. hello Data Lake tools lehetővé teszik tooeasily létrehozása, küldje el, és Azure hdinsight Hive-lekérdezések tooHadoop figyelése.

## <a id="prereq"></a>Előfeltételek

* (A HDInsight Hadoop) Azure HDInsight-fürtök

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* A Visual Studio (hello a következő verziók egyike):

    * A Visual Studio 2013 Community/Professional/Premium/Ultimate Update 4

    * A Visual Studio 2015 (minden kiadás)

    * A Visual Studio 2017 (minden kiadás)

* A HDInsight tools for Visual Studio vagy az Azure Data Lake tools for Visual Studio. Lásd: [első lépések a Visual Studio Hadoop tools for HDInsight használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) telepítésének és konfigurálásának hello eszközök információt.

## <a id="run"></a>Futtathat Hive-lekérdezéseket hello Visual Studio használatával

1. Nyissa meg **Visual Studio** válassza **új** > **projekt** > **Azure Data Lake** > **HIVE** > **Hive alkalmazás**. Adja meg a projekt nevét.

2. Nyissa meg hello **Script.hql** jön létre a projektet, és illessze be a következő HiveQL utasítások hello:

   ```hiveql
   set hive.execution.engine=tez;
   DROP TABLE log4jLogs;
   CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
   ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
   STORED AS TEXTFILE LOCATION '/example/data/';
   SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND  INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
   ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * `DROP TABLE`: Ha hello tábla létezik, a jelen nyilatkozatban törli őket.

   * `CREATE EXTERNAL TABLE`: Táblát hoz létre egy új "külső" struktúra. Külső táblák hello tábla definíciójában csak struktúra (hello adatok marad az eredeti helyükre hello) tárolja.

     > [!NOTE]
     > Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt. Például a MapReduce feladatot vagy Azure-szolgáltatás.
     >
     > A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.

   * `ROW FORMAT`: Közli a Hive hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.

   * `STORED AS TEXTFILE LOCATION`: Közli Hive adott hello tárolja (hello példaadatokat/könyvtár) és szövegként tárolt.

   * `SELECT`: Jelölje ki az összes sorok számát ahol oszlop `t4` hello értéket tartalmaz `[ERROR]`. A jelen nyilatkozat értéket ad vissza, `3` mert három ezt az értéket tartalmazó sorok.

   * `INPUT__FILE__NAME LIKE '%.log'`-Közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló. Ehhez a záradékhoz hello keresési toohello sample.log adatait tartalmazó fájl hello korlátozza.

3. Hello eszköztáron válassza a hello **HDInsight-fürt** toouse szeretné az ehhez a lekérdezéshez. Válassza ki **Submit** toorun hello utasításokra egy Hive-feladatot.

   ![Küldje el sáv](./media/hdinsight-hadoop-use-hive-visual-studio/toolbar.png)

4. Hello **Hive feladat összegzése** és hello feladat futtatásával kapcsolatos információkat jeleníti meg. Használjon hello **frissítése** toorefresh hello feladatinformációkat, amíg hello hivatkozás **feladat állapota** túl változik**befejezve**.

   ![a befejezett feladatok megjelenítése a feladat összegzése](./media/hdinsight-hadoop-use-hive-visual-studio/jobsummary.png)

5. Használjon hello **Job Output** tooview hello kimenetet a feldolgozás hivatkozásra. Megjeleníti `[ERROR] 3`, ez a lekérdezés által visszaadott hello értéket.

6. Hive-lekérdezéseket a projekt létrehozása nélkül is futtathatja. Használatával **Server Explorer**, bontsa ki a **Azure** > **HDInsight**, kattintson a jobb gombbal a HDInsight-kiszolgáló, és válassza **Hive lekérdezés írása**.

7. A hello **temp.hql** dokumentumot, amely akkor jelenik meg, vegye fel a következő HiveQL utasítások hello:

   ```hiveql
   set hive.execution.engine=tez;
   CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
   INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
   ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

   * `CREATE TABLE IF NOT EXISTS`: Létrehoz egy táblát, ha még nem létezik. Mivel hello `EXTERNAL` kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre. Belső tábla hello Hive adatraktár tárolódnak, és Hive kezeli.

     > [!NOTE]
     > Eltérően `EXTERNAL` táblák, egy belső tábla is eldobása hello alapjául szolgáló adatok törlése.

   * `STORED AS ORC`: A tárolók hello adatok optimalizált sor oszlopos (ORC) formátumban. ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.

   * `INSERT OVERWRITE ... SELECT`: Sorok kiválaszt hello `log4jLogs` tartalmazó `[ERROR]`, majd beszúrása adatok hello be hello `errorLogs` tábla.

8. Hello eszköztárán válassza **Submit** toorun hello feladat. Használjon hello **feladatállapot** toodetermine hello feladat sikeresen befejeződött.

9. amely a feladat létrehozva hello tábla, használjon hello tooverify **Server Explorer** csomópontot **Azure** > **HDInsight** > a HDInsight-fürt > **Hive adatbázisokat** > **alapértelmezett**. Hello **errorLogs** tábla és hello **log4jLogs** táblázatban vannak felsorolva.

## <a id="nextsteps"></a>Következő lépések

Ahogy látja, hello a HDInsight tools for Visual Studio egy egyszerűen toowork a Hive-lekérdezéseket a HDInsight adja meg.

Általános információk a hdinsight Hive:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

Más módszerekkel kapcsolatos információk a HDInsight Hadoop dolgozhat:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)

* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Hello kapcsolatos további információk a HDInsight tools for Visual Studio:

* [Kezdeti lépések a HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md)

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

[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png
