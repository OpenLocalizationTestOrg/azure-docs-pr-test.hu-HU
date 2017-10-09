---
title: "aaaWhat az Apache Hive és a HiveQL - Azure HDInsight |} Microsoft Docs"
description: "A Hadoop adatraktárrendszer Apache Hive. Lekérheti a Hive használata a HiveQL, mely hasonló tooTransact SQL adataihoz. Ebből a dokumentumból megtudhatja, hogyan toouse struktúra és az Azure HDInsight HiveQL."
keywords: "hiveql, mi az hive, hadoop hiveql, hogyan toouse struktúra, ismerje meg a hive, mi az hive"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2c10f989-7636-41bf-b7f7-c4b67ec0814f
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: a2772312263895ff99b499898264c2e6d5e816e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-apache-hive-and-hiveql-on-azure-hdinsight"></a>Mi az Apache Hive és a Azure HDInsight HiveQL?

[Apache Hive](http://hive.apache.org/) egy adatraktárrendszer van a Hadoop. Hive lehetővé teszi, hogy adatainak összefoglalója lekérdezése és az adatok elemzését. Hive-lekérdezések HiveQL, amely egy lekérdezési nyelv hasonló tooSQL nyelven íródtak.

Hive lehetővé teszi tooproject struktúra nagymértékben strukturálatlan adatokon. Hello struktúra meghatározása után HiveQL tooquery hello adatok Java vagy MapReduce ismerete nélkül is használhatja.

A HDInsight fürt számos különböző, amelyek adott munkaterhelés konkrét hangolt biztosít. a következő fürttípusok hello leggyakrabban használt a Hive-lekérdezéseket:

* __Interaktív Hive__: A Hadoop-fürt biztosító [alacsony késleltetésű analitikus feldolgozási (LLAP)](https://cwiki.apache.org/confluence/display/Hive/LLAP) interaktív lekérdezések funkció tooimprove válaszidejét. További információkért lásd: hello [interaktív Hive hdinsight kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.

* __Hadoop__: A Hadoop-fürt, amely a kötegelt feldolgozáson munkaterhelések van beállítva. További információkért lásd: hello [indítsa el a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md) dokumentum.

* __Spark__: Apache Spark rendelkezik beépített funkcióval Hive használata. További információkért lásd: hello [indítsa el a Spark on HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) dokumentum.

* __A HBase__: HiveQL lehet a HBase használt tooquery adataihoz. További információkért lásd: hello [indítsa el a HDInsight HBase](hdinsight-hbase-tutorial-get-started-linux.md) dokumentum.

## <a name="how-toouse-hive"></a>Hogyan toouse struktúra

A következő hogyan toouse hdinsight Hive tábla toodiscover hello használata:

| **Ezzel a módszerrel** Ha azt szeretné... | .. .an **interaktív** rendszerhéj | ... **kötegelt** feldolgozása | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél operációs rendszer** |
|:--- |:---:|:---:|:--- |:--- |
| [Hive nézete](hdinsight-hadoop-use-hive-ambari-view.md) |✔ |✔ |Linux |Bármely (böngészőalapú) |
| [Beeline ügyfél](hdinsight-hadoop-use-hive-beeline.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X vagy Windows |
| [REST API](hdinsight-hadoop-use-hive-curl.md) |&nbsp; |✔ |Linux vagy a Windows |Linux, Unix, Mac OS X vagy Windows |
| [A HDInsight tools for Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |&nbsp; |✔ |Linux vagy a Windows |Windows |
| [A Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md) |&nbsp; |✔ |Linux vagy a Windows |Windows |

> [!IMPORTANT]
> \*Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).
>
> Ha egy Windows-alapú HDInsight-fürtöt használ, használhatja a hello [lekérdezés konzol](hdinsight-hadoop-use-hive-query-console.md) a böngészőből vagy [távoli asztal](hdinsight-hadoop-use-hive-remote-desktop.md) toorun Hive-lekérdezéseket.

## <a name="hiveql-language-reference"></a>HiveQL nyelvi referencia

HiveQL nyelvi dokumentáció áll rendelkezésre a hello [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual).

## <a name="hive-and-data-structure"></a>Hive és az adatok szerkezete

Hive tisztában van azzal, hogyan toowork a strukturált és félig strukturált adatok. Például szövegfájlok ahol hello mezők határolja különleges karaktereket. a következő HiveQL utasítás hello táblázatot hoz létre szóközökkel elválasztott kötetnevek adatok:

```hiveql
CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
STORED AS TEXTFILE LOCATION '/example/data/';
```

Hive is támogatja az egyéni **szerializáló/deserializers (SerDe)** túl összetett vagy szabálytalan strukturált adatok számára. További információkért lásd: hello [hogyan toouse a hdinsightban egyéni JSON SerDe](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx) dokumentum.

A Hive támogatott fájlformátumok további információkért lásd: hello [nyelvi manuális (https://cwiki.apache.org/confluence/display/Hive/LanguageManual)](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)

## <a name="hive-internal-tables-vs-external-tables"></a>Belső tábla és a külső táblákra struktúra

Olyan táblázatot, amely a Hive hozhat létre két típusa van:

* __Belső__: hello Hive adatraktár adatokat tárolja. hello adatraktár itt található: `/hive/warehouse/` hello alapértelmezett tároló hello fürthöz.

    Belső használja táblák esetén:

    * Adatok csak átmenetileg létezik.
    * Azt szeretné, hogy a struktúra toomanage hello életciklus hello táblázat és az adatok.

* __Külső__: kívül hello adatraktár tárolja. hello adatok hello fürt által elérhető minden tárterület tárolható.

    Használjon külső táblák esetén:

    * hello adatok Hive kívül is használható. Például hello adatfájlok frissítése egy másik folyamat (vagyis nem zárolja hello fájlokat.)
    * Adatoknak az alapul szolgáló hely, hello tábla eldobása után is hello tooremain kell.
    * Egyéni helyen, például egy nem alapértelmezett tárfiók van szüksége.
    * Nem a hive kezeli a hello adatformátum hely, stb.

További információkért lásd: hello [Hive belső és külső táblák bevezetés] [ cindygross-hive-tables] blogbejegyzést.

## <a name="user-defined-functions-udf"></a>Felhasználói függvény (UDF)

Hive is kiterjeszthető keresztül **felhasználói függvény (UDF)**. Egy UDF lehetővé teszi tooimplement funkcionalitással kapcsolatban, vagy nem könnyen modellezve a HiveQL logika. Például egy felhasználó által megadott függvények használata a Hive tekintse meg a következő dokumentumok hello:

* [A Java-felhasználó által definiált függvény használata struktúra](hdinsight-hadoop-hive-java-udf.md)

* [A Python felhasználói függvény használata a Hive és a Pig használatával](hdinsight-python.md)

* [A C# felhasználó által definiált függvény használata Hive és a Pig használatával](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [A felhasználó által definiált egyéni struktúra tooadd tooHDInsight működési módját](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)

* [Egy példa Hive felhasználó által definiált függvény tooconvert dátum és idő formátumú tooHive időbélyeg](https://github.com/Azure-Samples/hdinsight-java-hive-udf)

## <a id="data"></a>Példa adatok

A HDInsight Hive előre betöltött tartalmaz egy belső tábla nevű `hivesampletable`. HDInsight Hive használható például adatkészleteket is biztosít. Ezek az adathalmazok tárolódnak hello `/example/data` és `/HdiSamples` könyvtárak. Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepel.

## <a id="job"></a>Példa Hive-lekérdezések

a következő HiveQL utasítások projektoszlopok alakzatot hello hello `/example/data/sample.log` fájlt:

    set hive.execution.engine=tez;
    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION '/example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

Hello előző példában hello HiveQL utasítások hajtsa végre a következő műveletek hello:

* `set hive.execution.engine=tez;`: Készletek hello végrehajtó motor toouse Tez. Tez helyett MapReduce biztosíthat a lekérdezési teljesítmény növelését. Tez további információkért lásd: hello [Apache Tez használja a jobb teljesítmény](#usetez) szakasz.

    > [!NOTE]
    > Az utasítás csak egy Windows-alapú HDInsight-fürt használata esetén szükséges. Tez hello alapértelmezett végrehajtó motorja a Linux-alapú HDInsight.

* `DROP TABLE`: Ha hello tábla már létezik, törölje azt.

* `CREATE EXTERNAL TABLE`: Létrehoz egy új **külső** Hive táblát. Külső táblák csak tárolása Hive hello tábla definíciójában. hello adatok marad az eredeti helyükre hello és hello eredeti formátumban.

* `ROW FORMAT`: Közli a Hive hello adatok formázását. Ebben az esetben az egyes naplókon hello mezők szóközzel elválasztva.

* `STORED AS TEXTFILE LOCATION`: Közli Hive adott hello tárolja (hello `example/data` könyvtár) és szövegként tárolt. hello adatok egyetlen fájlban vagy több fájl hello könyvtárban lévő elosztva.

* `SELECT`: Az összes sorok számát választ adott hello oszlop **t4** hello értéket tartalmaz **[hiba]**. A jelen nyilatkozat értéket ad vissza, **3** mert három ezt az értéket tartalmazó sorok.

* `INPUT__FILE__NAME LIKE '%.log'`-Hive megpróbál tooapply hello tooall sémafájlok hello könyvtárban. Hello directory ebben az esetben nem egyeznek meg a hello séma fájlokat tartalmazza. tooprevent szemétgyűjtési adatok hello eredmények között, a jelen nyilatkozat közli struktúra, hogy azt kell csak vissza adatokat fájlok végződése. napló.

> [!NOTE]
> Külső táblák kell használni, amikor hello frissíteni az külső forrás alapjául szolgáló adatok toobe várt. Például egy automatizált adatok feltöltési folyamat vagy MapReduce művelethez.
>
> A külső tábla eldobása does **nem** hello adatok törlése csak törli a hello tábla definíciójában.

toocreate egy **belső** helyett külső tábla, a következő HiveQL hello használata:

    set hive.execution.engine=tez;
    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

Ezekre az utasításokra hajtsa végre a következő műveletek hello:

* `CREATE TABLE IF NOT EXISTS`: Ha hello tábla nem létezik, hozza létre. Mivel hello **külső** kulcsszó nem használható, a jelen nyilatkozat egy belső táblát hoz létre. hello tábla hello Hive-adatraktárban tárolja, és Hive teljesen kezeli.

* `STORED AS ORC`: A hello adatot tárol a optimalizált sor oszlopos (ORC) formátumban. ORC formátuma egy magas optimalizált és hatékony Hive adatainak tárolásához.

* `INSERT OVERWRITE ... SELECT`: Sorok kiválaszt hello **log4jLogs** tartalmazó tábla **[hiba]**, majd beszúrása hello hello az adatok és **errorLogs** tábla.

> [!NOTE]
> Ellentétben a külső táblákhoz eldobását egy belső tábla is törli hello alapul szolgáló adatokat.

## <a name="improve-hive-query-performance"></a>Hive-lekérdezések teljesítményének növelése

### <a id="usetez"></a>Apache Tez

[Apache Tez](http://tez.apache.org) egy keretrendszer, amely lehetővé teszi az adatok alkalmazások, például a Hive, sokkal hatékonyabban léptékű toorun. A Linux-alapú HDInsight-fürtökön alapértelmezés szerint engedélyezve van a Tez.

> [!NOTE]
> Tez jelenleg ki alapértelmezés szerint a Windows-alapú HDInsight-fürtök és engedélyezni kell. Tez, a következő érték hello tootake előnyeit be kell állítani a Hive-lekérdezést:
>
> `set hive.execution.engine=tez;`
>
> Tez hello alapértelmezett motor a Linux-alapú HDInsight-fürtök.

Hello [Hive Tez tervezési dokumentumok](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) hello megvalósítási döntéseknek és hangolási konfigurációk részleteit tartalmazza.

a feladatok hibakeresés tooaid futott Tez használatával, a HDInsight lehetővé hello web UI, amelyek lehetővé teszik Tez feladatokhoz tooview részleteit a következő:

* [A Linux-alapú HDInsight Ambari Tez nézet hello használata](hdinsight-debug-ambari-tez-view.md)

* [A Windows-alapú HDInsight hello Tez felhasználói felület használata](hdinsight-debug-tez-ui.md)

### <a name="low-latency-analytical-processing-llap"></a>Kis késleltetésű analitikus feldolgozási (LLAP)

[LLAP](https://cwiki.apache.org/confluence/display/Hive/LLAP) (más néven hosszú Live és a folyamat), amely lehetővé teszi, hogy a memóriában történő gyorsítótárazás lekérdezések Hive 2.0 új szolgáltatása. LLAP teszi fel sokkal gyorsabb Hive-lekérdezések túl[26 x gyorsabb, mint a Hive 1.x bizonyos esetekben](https://hortonworks.com/blog/announcing-apache-hive-2-1-25x-faster-queries-much/).

A HDInsight fürt típusa interaktív Hive hello LLAP nyújt. További információkért lásd: hello [interaktív Hive kezdődnie](hdinsight-hadoop-use-interactive-hive.md) dokumentum.

## <a name="hive-jobs-and-sql-server-integration-services"></a>Hive-feladatok és az SQL Server Integration Services

Használhatja az SQL Server Integration Services (SSIS) toorun egy Hive-feladatot. hello Azure funkciócsomag SSIS a Hive-feladatok együttműködik a HDInsight összetevők a következő hello biztosít.

* [Az Azure HDInsight Hive feladat][hivetask]

* [Az Azure előfizetés Csatlakozáskezelő][connectionmanager]

További információk hello Azure funkciócsomag SSIS [Itt][ssispack].

## <a id="nextsteps"></a>Következő lépések

Most, hogy megismerte Hive van, és hogyan hadooppal a Hdinsightban, a következő használatát hello hivatkozik tooexplore más módokon toowork Azure hdinsightban toouse.

* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [MapReduce-feladatok használata a hdinsight eszközzel][hdinsight-use-mapreduce]

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx
