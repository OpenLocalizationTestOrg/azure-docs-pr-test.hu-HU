---
title: "Linux-alapú HDInsight Hadoop Oozie munkafolyamatok aaaUse |} Microsoft Docs"
description: "Linux-alapú HDInsight Hadoop Oozie használja. Megtudhatja, hogyan toodefine egy Oozie munkafolyamatot, és az Oozie feladat elküldéséhez."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d7603471-5076-43d1-8b9a-dbc4e366ce5d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: cb5682837543312621e3424b7a9341b5d2a00bf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a>Hadoop toodefine Oozie használjon, és a Linux-alapú HDInsight munkafolyamat futtatása

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Megtudhatja, hogyan toouse Hadoop on HDInsight az Apache Oozie. Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli. Oozie hello Hadoop-veremmel integrálva van, és a következő feladatok hello támogatja:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie akkor is, amelyek adott tooa rendszer, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok

> [!NOTE]
> A munkafolyamatok és a HDInsight együttes meghatározásához másik lehetőség is Azure Data Factory. További információ az Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie nincs engedélyezve a HDInsight-tartományhoz.

## <a name="prerequisites"></a>Előfeltételek

* **HDInsight-fürtök**: lásd: [HDInsight Linux első lépések](hdinsight-hadoop-linux-tutorial-get-started.md)

  > [!IMPORTANT]
  > hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="example-workflow"></a>Példa-munkafolyamat

a dokumentumban használt hello munkafolyamat két műveleteket tartalmaz. Műveletek végezhetők a definíciók feladatok, például a Hive, a Sqoop, a MapReduce vagy a másik folyamat fut:

![Munkafolyamat diagramja][img-workflow-diagram]

1. A Hive művelet a futás HiveQL-parancsfájlt tooextract rögzíti a hello **hivesampletable** HDInsight része. Minden egyes soraiban levő adatok egy adott mobileszközről meglátogatása ismerteti. hello rekord formátuma a következő szöveg hasonló toohello jelenik meg:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    hello ebben a dokumentumban használt Hive-parancsfájl megjeleníti az egyes platformokon (például Android vagy iPhone) hello teljes látogatások alkalmával, és hello számok tooa új Hive tábla tárolja.

    További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].

2. A Sqoop művelet hello hello új Hive tábla tooa-táblázat tartalmának Azure SQL adatbázis exportálja. További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].

> [!NOTE]
> Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello Hadoop-fürt verziók][hdinsight-versions].

## <a name="create-hello-working-directory"></a>Hozzon létre hello munkakönyvtár

Oozie vár a feladat toobe tárolt hello azonos szükséges erőforrások könyvtár. Ez a példa **wasb: / / / oktatóanyagok/useoozie**. Használja a következő parancs toocreate hello ez, és hello adatok könyvtárat, amely a hello a munkafolyamat által létrehozott új Hive tábla tárolja:

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> Hello `-p` paraméter hello elérési toobe létrehozott összes könyvtár okoz. Hello **adatok** könyvtár hello által használt használt toohold adatok **useooziewf.hql** parancsfájl.

A következő parancsot, amely biztosítja, hogy a Oozie megszemélyesíthet-e a felhasználói fiók Hive és a Sqoop feladat futtatásakor hello is futtathatja. Cserélje le **felhasználónév** a bejelentkezési névvel:

```
sudo adduser USERNAME users
```

> [!NOTE]
> Hibák figyelmen kívül hagyhatja, hogy hello a felhasználó tagja már hello `users` csoport.

## <a name="add-a-database-driver"></a>Egy adatbázis-illesztőprogram hozzáadása

Mivel ez a munkafolyamat Sqoop tooexport adatok tooSQL adatbázist használ, meg kell adnia, hello JDBC-illesztőt másolatát tootalk tooSQL adatbázis használja. Használjon hello parancs toocopy következő azt toohello munkakönyvtár:

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

Ha a munkafolyamat más erőforrások, például a MapReduce-alkalmazást tartalmazó jar, kellene tooadd ezeket az erőforrásokat is.

## <a name="define-hello-hive-query"></a>Hello Hive-lekérdezés megadása

A következő HiveQL-parancsfájlt, amely meghatározza az Oozie-munkafolyamat a dokumentum későbbi szakaszában a lekérdezés lépéseket toocreate hello használata.

1. Csatlakozzon az SSH használatával toohello fürt. hello parancsban látható egy példa hello segítségével `ssh` parancsot. Cserélje le __felhasználónév__ hello fürt hello SSH felhasználóval. Cserélje le __CLUSTERNAME__ hello nevű hello HDInsight-fürt.

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Hello SSH-kapcsolat alkalmazás a következő parancs toocreate fájl hello:

    ```
    nano useooziewf.hql
    ```

3. Amikor megnyílik a hello nano szerkesztő, használja a lekérdezés hello hello fájl tartalmát, a következő hello:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Nincsenek hello parancsfájl használt két változót:

    * **{hiveTableName} $**: hello tábla toobe létrehozott hello nevét tartalmazza

    * **{hiveDataFolder} $**: hello hely toostore hello adatfájlok hello tábla tartalmaz

    hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) továbbítja ezeket az értékeket toothis HiveQL-parancsfájlt futás közben

4. tooexit hello szerkesztő, nyomja le a Ctrl-X. Amikor a rendszer kéri, válassza ki a **Y** toosave hello fájlt, majd használja **Enter** toouse hello **useooziewf.hql** fájl nevét.

5. Használjon hello következő parancsok toocopy **useooziewf.hql** túl**wasb:///tutorials/useoozie/useooziewf.hql**:

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Ezek a parancsok tárolására hello **useooziewf.hql** hello fürt hello HDFS-kompatibilis tároló fájlt.

## <a name="define-hello-workflow"></a>Adja meg a hello munkafolyamat

Oozie munkafolyamatok definíciók hPDL (az XML folyamat Definition Language) nyelven íródtak. A következő lépéseket toodefine hello munkafolyamat hello használata:

1. A következő utasítás toocreate hello használja, és szerkeszthet egy új fájlt:

    ```
    nano workflow.xml
    ```

2. Ha hello nano szerkesztő nyit, adja meg a hello XML hello fájl tartalmát, a következő:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start too= "RunHiveScript"/>
        <action name="RunHiveScript">
        <hive xmlns="uri:oozie:hive-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.job.queue.name</name>
                <value>${queueName}</value>
            </property>
            </configuration>
            <script>${hiveScript}</script>
            <param>hiveTableName=${hiveTableName}</param>
            <param>hiveDataFolder=${hiveDataFolder}</param>
        </hive>
        <ok to="RunSqoopExport"/>
        <error to="fail"/>
        </action>
        <action name="RunSqoopExport">
        <sqoop xmlns="uri:oozie:sqoop-action:0.2">
            <job-tracker>${jobTracker}</job-tracker>
            <name-node>${nameNode}</name-node>
            <configuration>
            <property>
                <name>mapred.compress.map.output</name>
                <value>true</value>
            </property>
            </configuration>
            <arg>export</arg>
            <arg>--connect</arg>
            <arg>${sqlDatabaseConnectionString}</arg>
            <arg>--table</arg>
            <arg>${sqlDatabaseTableName}</arg>
            <arg>--export-dir</arg>
            <arg>${hiveDataFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\t"</arg>
            <archive>sqljdbc41.jar</archive>
            </sqoop>
        <ok to="end"/>
        <error to="fail"/>
        </action>
        <kill name="fail">
        <message>Job failed, error message[${wf:errorMessage(wf:lastErrorNode())}] </message>
        </kill>
        <end name="end"/>
    </workflow-app>
    ```

    Nincsenek definiált hello munkafolyamat két műveleteket:

   * **RunHiveScript**: Ez a művelet hello indítási műveletet, és futtatja a hello **useooziewf.hql** Hive parancsprogram

   * **RunSqoopExport**: Ez a művelet exportálja hello adatok hello Hive parancsfájl tooSQL alapján létrehozott adatbázis-Sqoop használatával. Ez a művelet csak akkor fut, ha hello **RunHiveScript** művelet befejeződött.

     hello munkafolyamat rendelkezik több bejegyzést, például a `${jobTracker}`. Ezek a bejegyzések a hello feladatdefinícióban használt értékek helyett. hello feladat meghatározása a jelen dokumentum jön létre.

     Megjegyzés: hello is `<archive>sqljdbc4.jar</arcive>` hello Sqoop szakasz bejegyzést. Ez a bejegyzés utasítja Oozie toomake ebből az archívumból érhető el a Sqoop Ez a művelet futtatásakor.

3. Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.

4. Használjon hello következő parancsot a toocopy hello **workflow.xml** fájl túl**/tutorials/useoozie/workflow.xml**:

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a>Hello adatbázis létrehozása

egy Azure SQL Database toocreate kövesse hello hello [SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md) dokumentum. Hello adatbázis létrehozásakor `oozietest` hello adatbázis neve. Továbbá jegyezze fel a hello hello adatbázis-kiszolgáló nevét.

### <a name="create-hello-table"></a>Hello tábla létrehozása

> [!NOTE]
> Nincsenek számos módon tooconnect tooSQL adatbázis toocreate egy tábla. a következő lépéseket használata hello [FreeTDS](http://www.freetds.org/) hello HDInsight-fürtök.


1. Parancs tooinstall FreeTDS következő hello HDInsight-fürt hello használata:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Amennyiben telepítve van a FreeTDS, használja a következő parancs tooconnect toohello SQL-adatbáziskiszolgáló korábban létrehozott hello:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    Kimeneti hasonló toohello a következő szöveg jelenik meg:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. A hello `1>` kéri, adja meg az alábbi hello:

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése. Ezekre az utasításokra, hozzon létre egy táblát nevű **mobiledata** hello munkafolyamat által használt.

    A következő tooverify, amelyek a tábla hello használata hello jött létre:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    Kimeneti hasonló toohello a következő szöveg jelenik meg:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.

## <a name="create-hello-job-definition"></a>Hello feladatdefiníció létrehozása

hello feladatdefiníció írja le, ahol toofind hello workflow.xml. Azt is bemutatja where toofind más hello munkafolyamat (például useooziewf.hql.) által használt fájlokat Azt is meghatározza hello értékek tulajdonságok hello munkafolyamaton belül használt, és a kapcsolódó fájlokat.

1. A következő parancs tooget hello teljes címe hello alapértelmezett tárolási hello használata. Ez a cím néhány percet szerepel hello konfigurációs fájlban:

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Ez a parancs visszaadja az adatokat hasonló toohello XML a következő:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Ha a HDInsight-fürt hello Azure Storage hello alapértelmezett tárolóként használ, hello `<value>` elem tartalma kezdődnie `wasb://`. Ha az Azure Data Lake Store használata esetén kezdődik `adl://`.

    Mentés hello hello tartalmát `<value>` elem, mert a hello lépések használatban van.

2. A következő parancs tooget hello fürt headnode teljes Tartományneve hello használata. Ez az információ hello JobTracker címet hello fürtnek szolgál:

    ```
    hostname -f
    ```

    Ez visszaadja a szöveg a következő információk hasonló toohello:

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    hello hello JobTracker használt port 8050, így a Hadoopból hello hello teljes címe toouse van `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.

3. A következő toocreate hello Oozie feladat definition konfigurációs hello használata:

    ```
    nano job.xml
    ```

4. Amikor megnyílik a hello nano szerkesztő, használja a hello XML hello hello fájl tartalmát, a következő:

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
        <name>nameNode</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
        </property>

        <property>
        <name>jobTracker</name>
        <value>JOBTRACKERADDRESS</value>
        </property>

        <property>
        <name>queueName</name>
        <value>default</value>
        </property>

        <property>
        <name>oozie.use.system.libpath</name>
        <value>true</value>
        </property>

        <property>
        <name>hiveScript</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/useooziewf.hql</value>
        </property>

        <property>
        <name>hiveTableName</name>
        <value>mobilecount</value>
        </property>

        <property>
        <name>hiveDataFolder</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/data</value>
        </property>

        <property>
        <name>sqlDatabaseConnectionString</name>
        <value>"jdbc:sqlserver://serverName.database.windows.net;user=adminLogin;password=adminPassword;database=oozietest"</value>
        </property>

        <property>
        <name>sqlDatabaseTableName</name>
        <value>mobiledata</value>
        </property>

        <property>
        <name>user.name</name>
        <value>YourName</value>
        </property>

        <property>
        <name>oozie.wf.application.path</name>
        <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
    </configuration>
    ```

   * Cserélje le az összes példányát  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  alapértelmezett tárolási korábban fogadott hello értékkel.

     > [!WARNING]
     > Ha hello elérési út egy `wasb` elérési útja, hello teljes elérési útja kell használnia. Nem rövidíthető az toojust `wasb:///`.

   * Cserélje le **JOBTRACKERADDRESS** a korábban fogadott JobTracker/ResourceManager cím hello.
   * Cserélje le **sajatNev** hello HDInsight-fürthöz, a bejelentkezési névvel.
   * Cserélje le **kiszolgálónév**, **adminLogin**, és **adminPassword** hello információkkal, illetve az Azure SQL Database adatbázishoz.

     A legtöbb hello információkat ebben a fájlban használt toopopulate hello értékek fájlokban használt hello workflow.xml vagy ooziewf.hql (például ${nameNode}.)

     > [!NOTE]
     > Hello **oozie.wf.application.path** a bejegyzés határozza meg, ha a feladat futott toofind hello workflow.xml fájl, amely tartalmazza a hello munkafolyamat.

5. Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.

## <a name="submit-and-manage-hello-job"></a>Küldje el, majd hello feladat kezelése

hello lépések használata hello Oozie parancs toosubmit és kezelése Oozie munkafolyamatok hello fürtön. hello Oozie parancs egy egyszerű felületen keresztül hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> Hello Oozie parancs használata esetén a hello FQDN a hello HDInsight headnode kell használnia. Ez a teljes tartománynév csak elérhető hello fürtből, vagy ha hello fürt hello a más számítógépről egy Azure virtuális hálózat ugyanazon a hálózaton.


1. A következő tooobtain hello URL-cím toohello Oozie szolgáltatás hello használata:

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Ez visszaad információk hasonló toohello XML a következő:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    Hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` részét hello URL-cím toouse hello Oozie parancsot a rendszer.

2. Következő hello URL toocreate egy környezeti változó, így nem kell tootype használata hello azt minden parancs:

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    Cserélje le egy korábban fogadott hello hello URL-CÍMÉT.
3. A következő toosubmit hello feladat hello használata:

    ```
    oozie job -config job.xml -submit
    ```

    A parancs betölt hello feladat adatait **job.xml** és elküldi tooOozie, de nem futtatható biztosítja.

    Hello parancs után az hello feladat hello Azonosítóját kell visszaadnia. Például: `0000005-150622124850154-oozie-oozi-W`. Ez az azonosító is használt toomanage hello feladat.

4. A következő parancs hello hello feladat hello állapotának megjelenítése:

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Cserélje le `<JOBID>` hello az azonosító hello előző lépés eredményeképpen visszakapott.

    Ez visszaadja a szöveg a következő információk hasonló toohello:

    ```
    Job ID : 0000005-150622124850154-oozie-oozi-W
    ------------------------------------------------------------------------------------------------------------------------------------
    Workflow Name : useooziewf
    App Path      : wasb:///tutorials/useoozie
    Status        : PREP
    Run           : 0
    User          : USERNAME
    Group         : -
    Created       : 2015-06-22 15:06 GMT
    Started       : -
    Last Modified : 2015-06-22 15:06 GMT
    Ended         : -
    CoordAction ID: -
    ------------------------------------------------------------------------------------------------------------------------------------
    ```

    Ez a feladat állapota `PREP`. Ez azt jelzi, hogy hello feladat lett létrehozva, de nem indult el.

5. A következő parancs toostart hello feladat hello használata:

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Cserélje le `<JOBID>` hello az azonosító adott vissza korábban.

    Ha hello állapotát a parancs után ellenőrizze, hogy futó állapotban van, és hello feladat hello műveleteire vonatkozó információk jelennek.

6. Hello feladat sikeres befejeződése után ellenőrizheti, hogy hello adatok jött létre, és toohello SQL-adatbázis táblája hello a következő parancsok futtatásával exportált:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    A hello `1>` kéri, adja meg a következő lekérdezés hello:

    ```
    SELECT * FROM mobiledata
    GO
    ```

    hello adatokat adott vissza a következő szöveg hasonló toohello:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

Hello Oozie parancs további információkért lásd: [Oozie parancssori eszköz](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API-n

hello Oozie REST API toobuild lehetővé teszi a saját eszközök Oozie együtt használható. Az alábbiakban hello HDInsight hello Oozie REST API használatával kapcsolatos információkat:

* **URI**: hello REST API-n elérhető külső hello fürthöz`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Hitelesítési**: toohello API hello fürt HTTP-fiókkal (rendszergazda) és a jelszó hitelesítéséhez. Példa:

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

Hello Oozie REST API használatával kapcsolatos további információkért lásd: [Oozie webszolgáltatási API-ra](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie webes felhasználói felület

hello Oozie webes felhasználói felület révén Oozie feladatok állapotának hello webes képet hello fürtön. hello webes felhasználói felület lehetővé teszi a következő információ tooview hello:

* Feladat állapota
* Feladatdefiníció
* Konfiguráció
* Hello műveletek hello feladat grafikonja
* Naplók hello feladat

Műveletek a feladat részleteit is megtekintheti.

tooaccess hello Oozie webes felhasználói felületén, a következő lépéseket hello használata:

1. Hozzon létre egy SSH-alagút toohello HDInsight-fürthöz. További információ: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

2. Ha alagút létrejött, a böngészőben nyissa meg a hello Ambari webes felhasználói felület. hello URI hello Ambari hely van **https://CLUSTERNAME.azurehdinsight.net**. Cserélje le **CLUSTERNAME** hello nevet, a Linux-alapú HDInsight-fürthöz.

3. Válassza ki a hello bal oldalán található hello, **Oozie**, majd **Gyorshivatkozások**, és végül **Oozie webes felhasználói felületén**.

    ![hello menük képe](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. hello Oozie webes felhasználói felületének alapértelmezett toodisplaying rendszert futtató munkafolyamat-feladatokat. toosee összes munkafolyamat-feladatokat, válassza ki **összes feladat**.

    ![Minden feladat jelenik meg](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Válassza ki a feladat tooview hello feladat további információt.

    ![Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. Hello Feladatinformáció lapján tekintheti meg egyszerű feladat információi és hello egyes műveletek hello feladat. Hello lapok használatával megtekintheti hello felső feladatdefiníció, konfigurációs feladatok, hozzáférési hello Job Log hello, vagy egy irányított aciklikus diagramhoz (DAG) hello feladat megtekintése.

   * **Job Log**: Select hello **GetLogs** tooget hello feladathoz tartozó összes naplófájlt, illetve programmal hello **keresési szűrőt adjon meg** toofilter naplók mezőben

       ![Feladat-napló](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **JobDAG**: hello DAG hello adatelérési utak hello munkafolyamaton keresztül végrehajtott grafikus áttekintése

       ![Feladat DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. Egy hello művelet kijelölésével hello **Feladatinformáció** lapon megjelenik információk hello a művelethez. Válassza például a hello **RunHiveScript** művelet.

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Láthatja a részletes hello műveletben, például egy hivatkozás toohello **konzol URL-címe**. Ez a hivatkozás használt tooview JobTracker információk hello feladat lehet.

## <a name="scheduling-jobs"></a>Feladatütemezés

hello koordinátor lehetővé teszi az start, záró és előfordulási gyakoriságát feladatok toospecify. toodefine hello munkafolyamat, a lépéseket követve használata hello ütemezés szerint:

1. A következő toocreate nevű fájl használatát hello **coordinator.xml**:

    ```
    nano coordinator.xml
    ```

    Hello XML hello hello fájl tartalmát, a következő használja:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
        <workflow>
            <app-path>${workflowPath}</app-path>
        </workflow>
        </action>
    </coordinator-app>
    ```

    > [!NOTE]
    > Hello `${...}` változók futásidőben hello feladatdefiníció értékek helyett. hello változók az alábbiak:
    >
    > * `${coordFrequency}`: Az hello feladat futó példányait között eltelt idő.
    > ** `${coordStart}`: hello feladat kezdési időpontja.
    > * `${coordEnd}`: hello feladat befejezési időpontja.
    > * `${coordTimezone}`: Koordinátor feladatok vannak rögzített időzónában nem nyári időszámításra (általában képviseli UTC használatával). Ez az időzóna kezeli hello "Oozie feldolgozási időzóna."
    > * `${wfPath}`: hello elérési toohello workflow.xml.

2. toosave hello fájl létrehozásához használja a Ctrl-X, **Y**, és **Enter**.

3. A következő parancs toocopy hello fájl toohello munkakönyvtár megadása a feladat hello használata:

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. A következő toomodify hello használata hello **job.xml** fájlt:

    ```
    nano job.xml
    ```

    Hajtsa végre a következő módosításokat hello:

   * tooinstruct oozie toorun hello koordinátor fájl helyett hello munkafolyamat, módosítás `<name>oozie.wf.application.path</name>` túl`<name>oozie.coord.application.path</name>`.

   * tooset hello `workflowPath` hello koordinátor által használt változó hozzáadása a következő XML hello:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Cserélje le a hello `wasb://mycontainer@mystorageaccount.blob.core.windows` hello értékkel más bejegyzések hello job.xml fájlban használt szöveg.

   * toodefine hello start, záró és hello koordinátor gyakorisága adja hozzá a következő XML hello:

        ```xml
        <property>
            <name>coordStart</name>
            <value>2017-05-10T12:00Z</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>2017-05-12T12:00Z</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>1440</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>UTC</value>
        </property>
        ```

       Ezek az értékek beállítása hello kezdési idő too12: 00 PM 2017. Előfordulhat, hogy 10., a befejezési idő tooMay 12, hello 2017. Ez a feladat futtatása naponta hello intervallumát. hello gyakorisága (percben), akkor úgy 24 óra x 60 perc = 1440 perc. Végezetül hello időzóna tooUTC van beállítva.

5. Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.

6. toorun hello feladat, a következő parancs használata hello:

    ```
    oozie job -config job.xml -run
    ```

    Ez a parancs küldi el, és elindítja hello feladatot.

7. Ha látogasson el a hello Oozie webes felhasználói felületén, és válassza ki a hello **koordinátor feladatok** lapon látható információk hasonló toohello kép a következő:

    ![koordinátor feladatok lap](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    Hello **következő Materialization** -bejegyzése tartalmazza hello, amikor legközelebb hello feladat futtatása.

8. Hasonló toohello korábbi munkafolyamat-feladat, kiválasztja hello feladat bejegyzés hello webes felhasználói felület hello feladattal kapcsolatos információkat jeleníti meg:

    ![Koordinátor Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Ez a rendszerkép csak hello feladat sikeres futtatása ütemezett hello munkafolyamaton belül nem az egyéni műveletek jeleníti meg. Ezt követően válasszon ki egy hello toosee **művelet** bejegyzéseket.

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Hibaelhárítás

hello Oozie felhasználói felület lehetővé teszi a tooview Oozie naplókat. Hivatkozások tooJobTracker naplók hello munkafolyamat indította MapReduce-feladatok is tartalmaz. hibaelhárítási hello mintát kell lennie:

1. Hello feladat megtekintése Oozie webes felhasználói felületén.

2. Ha egy hiba vagy egy adott művelet sikertelen, válassza ki a művelet toosee hello, ha hello **hibaüzenet** mező hello hiba nyújt részletesebb információt.

3. Ha elérhető, használja hello művelet tooview hello URL-CÍMÉT (például a Hadoopból naplók) további részleteket hello a művelethez.

hello az alábbiakban felmerülhet, bizonyos hibákat, és hogyan tooresolve őket.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Nem lehet inicializálni a fürt

**A jelenség**: hello túl a feladat állapotának megváltozásakor**FELFÜGGESZTETT**. Hello feladat részleteinek megjelenítése hello RunHiveScript állapotának **START_MANUAL**. Látható értesítések valamelyikének kiválasztásakor hello művelet a következő hibaüzenet hello:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**OK**: hello WASB címekre hello **job.xml** fájl tartalmaz hello tároló vagy a tárfiók nevét. hello WASB cím formátumban kell megadni `wasb://containername@storageaccountname.blob.core.windows.net`.

**Megoldási**: hello feladat által használt hello WASB címek módosítása.

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a>JA002: A Oozie nem engedélyezett a tooimpersonate &lt;felhasználó >

**A jelenség**: hello túl a feladat állapotának megváltozásakor**FELFÜGGESZTETT**. Hello feladat részleteinek megjelenítése hello RunHiveScript állapotának **START_MANUAL**. A következő hibaüzenet hello hello művelet kiválasztása jeleníti meg:

    JA002: User: oozie is not allowed tooimpersonate <USER>

**OK**: aktuális engedély-beállítások nem teszik lehetővé az Oozie tooimpersonate hello adott felhasználói fiókot.

**Megoldási**: Oozie tooimpersonate felhasználók engedélyezett hello **felhasználók** csoport. Használjon hello `groups USERNAME` , amely a felhasználói fiók hello toosee hello csoportok tagja. Ha hello felhasználó nem tagja hello **felhasználók** csoportjában a következő parancs toohello tooadd hello felhasználócsoport hello használata:

    sudo adduser USERNAME users

> [!NOTE]
> HDInsight felismeri, hogy a felhasználó hello hozzáadott toohello csoport több percig is eltarthat.

### <a name="launcher-error-sqoop"></a>Hiba (Sqoop) indítója

**A jelenség**: hello túl a feladat állapotának megváltozásakor**KILLED**. Hello feladat részleteinek megjelenítése hello RunSqoopExport állapotának **hiba**. A következő hibaüzenet hello hello művelet kiválasztása jeleníti meg:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**OK**: Sqoop értéke nem lehet tooload hello illesztőprogram szükséges tooaccess hello adatbázist.

**Megoldási**: Sqoop egy Oozie feladatból használatakor meg kell adni hello adatbázis illesztőprogram hello más erőforrások (például a hello workflow.xml) hello feladat használja. Hello archív hello adatbázis illesztőprogram hello tartalmazó is hivatkozni `<sqoop>...</sqoop>` hello workflow.xml szakasza.

Például hello feladat ebben a dokumentumban, használja a következő lépéseket hello:

1. Másolja a hello sqljdbc4.1.jar toohello /tutorials/useoozie könyvtára:

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Módosítsa a hello workflow.xml tooadd hello XML a fenti új sor a következő `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat és hogyan toorun egy Oozie feladat. További információk a HDInsight, használata toolearn lásd: a következő cikkek hello:

* [Időalapú Oozie-koordinátor használata a hdinsight eszközzel][hdinsight-oozie-coordinator-time]
* [Hdinsight Hadoop-feladatokat az adatok feltöltése][hdinsight-upload-data]
* [Sqoop használata a hadooppal a Hdinsightban][hdinsight-use-sqoop]
* [A Hive használata a hdinsight Hadoop][hdinsight-use-hive]
* [A Pig használata a HDInsight Hadoop][hdinsight-use-pig]
* [Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563
[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started]: hdinsight-get-started.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop-mac-linux.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-use-blob-storage.md
[hdinsight-get-started-emulator]: hdinsight-get-started-emulator.md
[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: sql-database-create-configure.md
[sqldatabase-get-started]: sql-database-get-started.md

[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: https://technet.microsoft.com/en-us/library/ee176961.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie/HDI.UseOozie.RunWF.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
