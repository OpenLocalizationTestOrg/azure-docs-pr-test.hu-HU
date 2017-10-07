---
title: az Apache Hive - Azure HDInsight Beeline aaaUse |} Microsoft Docs
description: "Ismerje meg, hogyan toouse hello Beeline ügyfél toorun Hive HDInsight a hadooppal lekérdezi. Beeline olyan eszköz, amellyel a hiveserver2-n keresztül JDBC használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "beeline struktúra, hive beeline"
ms.assetid: 3adfb1ba-8924-4a13-98db-10a67ab24fca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: e788ff39f33d928808cfcb83a92f62ac9ae8ca09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-beeline-client-with-apache-hive"></a>Apache Hive hello Beeline ügyfél használata

Megtudhatja, hogyan toouse [Beeline](https://cwiki.apache.org/confluence/display/Hive/HiveServer2+Clients#HiveServer2Clients-Beeline–NewCommandLineShell) toorun Hive hdinsight lekérdezi.

Beeline szerepel-e a HDInsight-fürt hello átjárócsomópontokkal Hive ügyfél. Beeline JDBC tooconnect tooHiveServer2, egy a HDInsight fürt által futtatott szolgáltatást használ. Is használhatja Beeline tooaccess Hive hdinsight távolról több mint hello internet. a következő táblázat hello kapcsolati karakterláncok Beeline való használatra biztosítja:

| A Beeline futtató | Paraméterek |
| --- | --- | --- |
| Egy SSH kapcsolat tooa headnode vagy peremhálózati csomópont | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
| Külső hello fürt | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

> [!NOTE]
> Cserélje le `admin` hello fürt bejelentkezési fiókkal a fürt számára.
>
> Cserélje le `password` hello fürt bejelentkezési fiók hello jelszóval.
>
> Cserélje le `clustername` hello nevet, a HDInsight-fürthöz.

## <a id="prereq"></a>Előfeltételek

* A Linux-based Hadoop on HDInsight-fürt.

  > [!IMPORTANT]
  > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Egy SSH-ügyfél vagy a helyi Beeline ügyfél. A legtöbb hello jelen dokumentumban leírt lépések azt feltételezik, hogy használt Beeline SSH munkamenet toohello fürtök. A külső hello fürtből Beeline futó információkért lásd: hello [Beeline távolról használható](#remote) szakasz.

    További információ az SSH használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a id="beeline"></a>Beeline használata

1. Beeline indításakor kell megadnia egy kapcsolati karakterláncot a hiveserver2-n a HDInsight-fürtre. toorun hello parancs külső hello fürtből, meg kell adni hello fürt bejelentkezési fiók neve (alapértelmezett `admin`) és a jelszót. A következő tábla toofind hello kapcsolati karakterlánc formátuma és paraméterek toouse hello használata:

    | A Beeline futtató | Paraméterek |
    | --- | --- | --- |
    | Egy SSH kapcsolat tooa headnode vagy peremhálózati csomópont | `-u 'jdbc:hive2://headnodehost:10001/;transportMode=http'` |
    | Külső hello fürt | `-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2' -n admin -p password` |

    Például a következő parancs hello lehet használt toostart Beeline SSH munkamenet toohello fürtök:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    A parancs hello Beeline ügyfél elindul, és a hello átjárócsomóponthoz tooHiveServer2 csatlakozik. Hello parancs után megérkezik a egy `jdbc:hive2://headnodehost:10001/>` kérdés.

2. Beeline parancsok kezdődhet egy `!` karakter, például `!help` megjeleníti a súgót. Azonban hello `!` egyes parancsok kihagyható. Például `help` is működik.

    Nincs olyan `!sql`, mely van használt tooexecute HiveQL utasításokat. Azonban HiveQL ezért általában arra használják, akkor kihagyhatja az előző hello `!sql`. a következő két utasítások hello egyenértékűek:

    ```hiveql
    !sql show tables;
    show tables;
    ```

    Az új fürtön csak egy tábla szerepel: **hivesampletable**.

3. A következő parancs toodisplay hello séma hello hivesampletable hello használata:

    ```hiveql
    describe hivesampletable;
    ```

    Ez a parancs visszaadja a következő információ hello:

        +-----------------------+------------+----------+--+
        |       col_name        | data_type  | comment  |
        +-----------------------+------------+----------+--+
        | clientid              | string     |          |
        | querytime             | string     |          |
        | market                | string     |          |
        | deviceplatform        | string     |          |
        | devicemake            | string     |          |
        | devicemodel           | string     |          |
        | state                 | string     |          |
        | country               | string     |          |
        | querydwelltime        | double     |          |
        | sessionid             | bigint     |          |
        | sessionpagevieworder  | bigint     |          |
        +-----------------------+------------+----------+--+

    Ez a témakör hello hello tábla oszlopai. Most tudta elvégezni egyes lekérdezések adatok alapján, amíg inkább hozzon létre egy új tábla toodemonstrate hogyan tooload adatokat a Hive és a séma alkalmazása.

4. Adja meg a következő utasítások toocreate nevű tábla hello **log4jLogs** hello HDInsight-fürthöz megadott mintaadatokat használatával:

    ```hiveql
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;
    ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

    * `DROP TABLE`-Ha hello tábla létezik, akkor az törlődik.

    * `CREATE EXTERNAL TABLE`-Hoz létre egy **külső** Hive táblát. Külső táblák csak tárolása Hive hello tábla definíciójában. hello adatok hello eredeti helyen marad.

    * `ROW FORMAT`-Hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.

    * `STORED AS TEXTFILE LOCATION`-Ha hello tárolja, és milyen formátumban.

    * `SELECT`-Választja ki az összes sorok számát ahol oszlop **t4** hello értéket tartalmaz **[hiba]**. A lekérdezés által visszaadott érték **3** ahány három ezt az értéket tartalmazó sorok.

    * `INPUT__FILE__NAME LIKE '%.log'`-Hive megpróbál tooapply hello tooall sémafájlok hello könyvtárban. Hello directory ebben az esetben nem egyeznek meg a hello séma fájlokat tartalmazza. tooprevent szemétgyűjtési adatok hello eredmények között, a jelen nyilatkozat közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.

  > [!NOTE]
  > Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt. Például egy automatizált adatok feltöltési folyamat vagy a MapReduce művelethez.
  >
  > A külső tábla eldobása does **nem** törölni hello adatokat, csak a hello tábla definíciójában.

    hello kimeneti, a parancs van hasonló toohello a következő szöveget:

        INFO  : Tez session hasn't been created yet. Opening session
        INFO  :

        INFO  : Status: Running (Executing on YARN cluster with App id application_1443698635933_0001)

        INFO  : Map 1: -/-      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0/1      Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 0(+1)/1  Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0/1
        INFO  : Map 1: 1/1      Reducer 2: 0(+1)/1
        INFO  : Map 1: 1/1      Reducer 2: 1/1
        +----------+--------+--+
        |   sev    | count  |
        +----------+--------+--+
        | [ERROR]  | 3      |
        +----------+--------+--+
        1 row selected (47.351 seconds)

5. tooexit Beeline, használjon `!exit`.

## <a id="file"></a>Beeline toorun HiveQL fájl használata

A következő lépéseket toocreate egy fájlt, majd futtassa azt Beeline segítségével hello használata.

1. Használjon hello következő parancsot a toocreate nevű fájlt **query.hql**:

    ```bash
    nano query.hql
    ```

2. Szöveg hello hello fájl tartalmát, a következő hello használata. Ez a lekérdezés táblázatot hoz létre új "belső" nevű **errorLogs**:

    ```hiveql
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';
    ```

    Ezekre az utasításokra hajtsa végre a következő műveletek hello:

    * **Hozzon létre Ha nem létezik táblázat** -hello tábla még nem létezik, ha a rendszer létrehozza. Hello óta **külső** kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre. Belső tábla hello Hive adatraktár tárolódnak, és Hive teljesen kezeli.
    * **TÁROLT AS ORC** -hello eltárolja optimalizált sor oszlopos (ORC) formátumban. ORC Hive adattárolás magas optimalizált és hatékony formátuma érvénytelen.
    * **ÍRJA FELÜL AZ INSERT... Válassza ki** -sorok kiválaszt hello **log4jLogs** tartalmazó **[hiba]**, majd Beszúrások adatok hello hello be **errorLogs** tábla.

    > [!NOTE]
    > Külső táblák eltérően eldobását egy belső tábla törlése hello, valamint az alapul szolgáló adatokat.

3. toosave hello fájl használata **Ctrl**+**_X**, majd adja meg **Y**, és végül **Enter**.

4. Használja a következő toorun hello fájl használatával Beeline hello:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http' -i query.hql
    ```

    > [!NOTE]
    > Hello `-i` paraméter Beeline elindul, a futtatása hello utasítások hello query.hql fájlban. Miután hello lekérdezés befejeződött, megérkezik a hello `jdbc:hive2://headnodehost:10001/>` kérdés. Is futtathatja egy fájlt, hello `-f` paraméter, amely Beeline kilép hello lekérdezés befejeződése után.

5. amely hello tooverify **errorLogs** táblázat létrejött, használja a következő utasítás tooreturn összes sorát hello hello **errorLogs**:

    ```hiveql
    SELECT * from errorLogs;
    ```

    Három sor adat vissza kell adni, az összes tartalmazó **[hiba]** az oszlop t4:

        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | errorlogs.t1  | errorlogs.t2  | errorlogs.t3  | errorlogs.t4  | errorlogs.t5  | errorlogs.t6  | errorlogs.t7  |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        | 2012-02-03    | 18:35:34      | SampleClass0  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 18:55:54      | SampleClass1  | [ERROR]       | incorrect     | id            |               |
        | 2012-02-03    | 19:25:27      | SampleClass4  | [ERROR]       | incorrect     | id            |               |
        +---------------+---------------+---------------+---------------+---------------+---------------+---------------+--+
        3 rows selected (1.538 seconds)

## <a id="remote"></a>Beeline távolról használható

Ha helyileg telepített Beeline, vagy használja a Docker-lemezkép használatával, mint [sutoiku/beeline](https://hub.docker.com/r/sutoiku/beeline/), a következő paraméterek hello kell használnia:

* __Kapcsolati karakterlánc__:`-u 'jdbc:hive2://clustername.azurehdinsight.net:443/;ssl=true;transportMode=http;httpPath=/hive2'`

* __Fürt bejelentkezési neve__:`-n admin`

* __Fürt bejelentkezési jelszó__`-p 'password'`

Cserélje le a hello `clustername` a HDInsight-fürt hello nevű hello kapcsolódási karakterláncban.

Cserélje le `admin` hello nevet, a fürt bejelentkezési és a név felülírandó `password` hello jelszóval a fürt bejelentkezési azonosítóhoz.

## <a id="sparksql"></a>Beeline használata Spark

Spark hiveserver2-n, ami általában beruházások tooas hello Spark Thrift-kiszolgáló a saját végrehajtásának biztosít. Ez a szolgáltatás az Spark SQL tooresolve lekérdezések Hive helyett, és attól függően, hogy a lekérdezés jobb teljesítményt biztosíthat.

tooconnect toohello Spark Thrift-kiszolgáló, a Spark on HDInsight-fürt használata port `10002` helyett `10001`. Például: `beeline -u 'jdbc:hive2://headnodehost:10002/;transportMode=http'`.

> [!IMPORTANT]
> hello Spark Thrift-kiszolgáló nem érhető el over internet hello közvetlen van. Csak tooit csatlakozzon az SSH-munkamenetet, vagy belül hello azonos Azure virtuális hálózatban, a HDInsight-fürt hello.

## <a id="summary"></a><a id="nextsteps"></a>További lépések

A Hive HDInsight további általános információkért tekintse meg a következő dokumentum hello:

* [A Hive használata a hdinsight Hadoop](hdinsight-use-hive.md)

A HDInsight Hadoop dolgozhat egyéb lehetőségeiről további információkért lásd: a következő dokumentumok hello:

* [A Pig használata a HDInsight Hadoop](hdinsight-use-pig.md)
* [A HDInsight Hadoop MapReduce használata](hdinsight-use-mapreduce.md)

Ha Tez Hive használ, tekintse meg a következő dokumentumok hello:

* [A Windows-alapú HDInsight hello Tez felhasználói felület használata](hdinsight-debug-tez-ui.md)
* [A Linux-alapú HDInsight Ambari Tez nézet hello használata](hdinsight-debug-ambari-tez-view.md)

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

[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html


[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md


[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx
