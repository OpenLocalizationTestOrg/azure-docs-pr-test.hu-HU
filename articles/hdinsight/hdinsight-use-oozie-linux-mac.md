---
title: "Linux-alapú hdinsight Hadoop Oozie munkafolyamatok használata |} Microsoft Docs"
description: "Linux-alapú HDInsight Hadoop Oozie használja. Megtudhatja, hogyan határozza meg az Oozie munkafolyamat, valamint az Oozie feladat elküldéséhez."
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
ms.openlocfilehash: e3206078e451aefe02689bfb61ce22a20dd0fa70
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a>Határozza meg, és egy munkafolyamat futtatása a Linux-alapú HDInsight Hadoop Oozie használata

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

Útmutató Apache Oozie használata a HDInsight Hadoop. Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli. Oozie integrálva van a Hadoop-veremmel, és támogatja a következő feladatokat:

* Apache MapReduce
* Apache Pig
* Apache Hive
* Apache Sqoop

Oozie is használható, például Java programok vagy héjparancsfájlok ütemezésére rendszerspecifikus feladatok ütemezése

> [!NOTE]
> A munkafolyamatok és a HDInsight együttes meghatározásához másik lehetőség is Azure Data Factory. Azure Data Factory kapcsolatos további információkért lásd: [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].

> [!IMPORTANT]
> Oozie nincs engedélyezve a HDInsight-tartományhoz.

## <a name="prerequisites"></a>Előfeltételek

* **HDInsight-fürtök**: lásd: [HDInsight Linux első lépések](hdinsight-hadoop-linux-tutorial-get-started.md)

  > [!IMPORTANT]
  > A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="example-workflow"></a>Példa-munkafolyamat

A munkafolyamat a dokumentumban használt két műveleteket tartalmaz. Műveletek végezhetők a definíciók feladatok, például a Hive, a Sqoop, a MapReduce vagy a másik folyamat fut:

![Munkafolyamat diagramja][img-workflow-diagram]

1. A Hive művelet a futás bejegyzéseit kibontása a HiveQL-parancsfájlt a **hivesampletable** HDInsight része. Minden egyes soraiban levő adatok egy adott mobileszközről meglátogatása ismerteti. A rekord formátumban jelenik meg az alábbihoz hasonló:

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    Ebben a dokumentumban használt Hive-parancsfájl megjeleníti az egyes platformokon (például Android vagy iPhone) teljes látogatások, és a számát, az új Hive tábla tárolja.

    További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].

2. A Sqoop művelet a tartalmát az új Hive tábla exportálása egy Azure SQL adatbázis egyik táblája. További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].

> [!NOTE]
> Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított Hadoop-fürt verziók][hdinsight-versions].

## <a name="create-the-working-directory"></a>A munkakönyvtár létrehozása

Oozie vár egy feladatot, amely ugyanabban a könyvtárban kell tárolni szükséges erőforrásokat. Ez a példa **wasb: / / / oktatóanyagok/useoozie**. A következő paranccsal ez, és az adatok könyvtárat, amely tárolja az ezen munkafolyamat által létrehozott új Hive tábla létrehozásához:

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> A `-p` paraméter okoz az összes könyvtár elérési útját hozható létre. A **adatok** által használt adatok tárolására használt könyvtárat a **useooziewf.hql** parancsfájl.

Is futtassa a következő parancsot, amely biztosítja, hogy a Oozie megszemélyesíthet-e a felhasználói fiók Hive és a Sqoop feladat futtatásakor. Cserélje le **felhasználónév** a bejelentkezési névvel:

```
sudo adduser USERNAME users
```

> [!NOTE]
> A hibákat, a felhasználó már tagja figyelmen kívül hagyhatja a `users` csoport.

## <a name="add-a-database-driver"></a>Egy adatbázis-illesztőprogram hozzáadása

Mivel ez a munkafolyamat Sqoop használ az adatok exportálása az SQL Database, meg kell adnia egy példányát az SQL-adatbázis felvegye használt JDBC-illesztőt. Az alábbi parancs segítségével másolja azt a munkakönyvtárat:

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

Ha a munkafolyamat más erőforrások, például a MapReduce-alkalmazást tartalmazó jar használ, adja hozzá ezeket az erőforrásokat, valamint kellene.

## <a name="define-the-hive-query"></a>A Hive-lekérdezés megadása

A következő lépésekkel HiveQL-parancsfájlt, amely meghatározza az Oozie-munkafolyamat a dokumentum későbbi szakaszában a lekérdezés létrehozásához.

1. Csatlakozzon a fürthöz SSH használatával. A következő parancsot az használatának példája a `ssh` parancsot. Cserélje le __felhasználónév__ a fürthöz az SSH felhasználóval. Cserélje le __CLUSTERNAME__ a HDInsight-fürt nevét.

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Az SSH-kapcsolat alkalmazás-fájl létrehozásához a következő parancsot:

    ```
    nano useooziewf.hql
    ```

3. Megnyílik a nano szerkesztő, ha a fájl tartalmát a következő lekérdezés használata:

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    Nincsenek a parancsfájl használt két változót:

    * **{hiveTableName} $**: hozható létre a tábla nevét tartalmazza

    * **{hiveDataFolder} $**: a tábla az adatforrás adatfájljainak tárolási helyét tartalmazza

    A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) továbbítja ezeket az értékeket a HiveQL-parancsfájlt futás közben

4. Zárja be a szerkesztőt, nyomja le a Ctrl-X. Amikor a rendszer kéri, válassza ki a **Y** mentse a fájlt, majd használja **Enter** használata a **useooziewf.hql** fájl nevét.

5. Másolja az alábbi parancsokkal **useooziewf.hql** való **wasb:///tutorials/useoozie/useooziewf.hql**:

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    Ezek a parancsok tárolja a **useooziewf.hql** a fürt a HDFS-kompatibilis tároló fájlt.

## <a name="define-the-workflow"></a>Adja meg a munkafolyamat

Oozie munkafolyamatok definíciók hPDL (az XML folyamat Definition Language) nyelven íródtak. Az alábbi lépések segítségével határozza meg a munkafolyamat:

1. A következő utasítás használatával hozzon létre és szerkeszthet egy új fájlt:

    ```
    nano workflow.xml
    ```

2. Megnyílik a nano szerkesztő, miután adja meg a következő XML fájl tartalma:

    ```xml
    <workflow-app name="useooziewf" xmlns="uri:oozie:workflow:0.2">
        <start to = "RunHiveScript"/>
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

    A munkafolyamatban meghatározott két tevékenységek vannak:

   * **RunHiveScript**: Ez a művelet az indítási műveletet, és futtatja a **useooziewf.hql** Hive parancsprogram

   * **RunSqoopExport**: Ez a művelet exportálja az adatokat az SQL Database szolgáltatásba Sqoop a Hive-parancsfájl alapján létrehozott. Ez a művelet csak akkor fut, ha a **RunHiveScript** művelet befejeződött.

     A munkafolyamat rendelkezik több bejegyzést, például a `${jobTracker}`. Ezek a bejegyzések feladatdefinícióban használt értékek helyett. A feladat meghatározása a jelen dokumentum jön létre.

     Azt is vegye figyelembe a `<archive>sqljdbc4.jar</arcive>` a Sqoop szakasz bejegyzése. Ez a bejegyzés arra utasítja az Oozie elérhetővé ebből az archívumból Sqoop számára ez a művelet futtatásakor.

3. Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.

4. Másolja az alábbi parancs segítségével a **workflow.xml** fájl **/tutorials/useoozie/workflow.xml**:

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-the-database"></a>Az adatbázis létrehozása

Az Azure SQL adatbázis létrehozásához kövesse a [SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md) dokumentum. Az adatbázis létrehozásakor `oozietest` az adatbázis neve. Is jegyezze fel az adatbázis-kiszolgáló nevét.

### <a name="create-the-table"></a>A tábla létrehozása

> [!NOTE]
> Számos módon hozzon létre egy táblát az SQL-adatbázishoz való kapcsolódáshoz. Az alábbi lépéseket használata [FreeTDS](http://www.freetds.org/) a a HDInsight-fürthöz.


1. A következő paranccsal FreeTDS a HDInsight-fürt telepítése:

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. Egyszer FreeTDS telepítve van, ezért a korábban létrehozott SQL Database-kiszolgálóhoz való kapcsolódáshoz a következő paranccsal:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    A kimenet az alábbihoz hasonló jelenhet meg:

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. : A `1>` kéri, adja meg a következő sorokat:

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    Ha a `GO` utasításban is meg kell adni, az előző utasítások kiértékelése. Ezekre az utasításokra, hozzon létre egy táblát nevű **mobiledata** a munkafolyamat által használt.

    A következő segítségével győződjön meg arról, hogy a tábla jött létre:

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    A kimenet az alábbihoz hasonló jelenik meg:

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. Adja meg `exit` , a `1>` Rákérdezés a tsql segédprogram kilép.

## <a name="create-the-job-definition"></a>A feladat definíciójának létrehozása

A feladat definíciójához hol található a workflow.xml ismerteti. Azt is ismerteti, hol található egyéb fájlok (például useooziewf.hql.) a munkafolyamat által használt Azt is meghatározza az értékek tulajdonságok a munkafolyamaton belül használt, és a kapcsolódó fájlokat.

1. A következő paranccsal alapértelmezett tárterület teljes címét. Ez a cím néhány percet szerepel a konfigurációs fájlban:

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    Ez a parancs információkat ad vissza a következő XML-hasonló:

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > Ha a HDInsight-fürt Azure a tárolót használja az alapértelmezett tárolóként a `<value>` elem tartalma kezdődnie `wasb://`. Ha az Azure Data Lake Store használata esetén kezdődik `adl://`.

    Menti a `<value>` elem, mert használatban van a következő lépéseket.

2. A következő paranccsal lekérni a fürt headnode teljes Tartományneve. Ezt az információt a Hadoopból címet a fürtnek szolgál:

    ```
    hostname -f
    ```

    Ez hasonló ad vissza adatokat a következő szöveget:

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    A Hadoopból használt port 8050,, így a Hadoopból használt a teljes címet van `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.

3. Az Oozie feladat definition konfiguráció létrehozásához használja a következőket:

    ```
    nano job.xml
    ```

4. Amikor megnyílik a nano szerkesztő, használja a következő XML, a fájl tartalmát:

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

   * Cserélje le az összes példányát  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  alapértelmezett tárolási korábban fogadott értékkel.

     > [!WARNING]
     > Ha az elérési út egy `wasb` elérési útja, a teljes elérési útja kell használnia. Nem rövidíthető úgy, hogy csak `wasb:///`.

   * Cserélje le **JOBTRACKERADDRESS** korábban fogadott JobTracker/ResourceManager címmel.
   * Cserélje le **Saját_név** a bejelentkezési névvel a HDInsight-fürthöz.
   * Cserélje le **kiszolgálónév**, **adminLogin**, és **adminPassword** és az Azure SQL Database vonatkozó információkat.

     A legtöbb a fájlban található információ a workflow.xml vagy ooziewf.hql fájlokat (például ${nameNode}.) a használt értékek feltöltésére használt eszköz

     > [!NOTE]
     > A **oozie.wf.application.path** a bejegyzés határozza meg, hol található a workflow.xml fájl, a feladat futott, amely tartalmazza a munkafolyamat.

5. Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.

## <a name="submit-and-manage-the-job"></a>Küldje el, majd a feladatok kezelése

Az alábbi lépéseket az Oozie-parancs segítségével küldje el, majd a fürt Oozie munkafolyamatok kezelése. A Oozie parancs nem egy egyszerű felületen keresztül a [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

> [!IMPORTANT]
> Az Oozie-paranccsal, ha a HDInsight headnode a teljes Tartománynevét kell használnia. Ez a teljes tartománynév csak az elérhető a fürtből, vagy ha a fürt egy Azure virtuális hálózat ugyanazon a hálózaton lévő más gépekkel.


1. Használja a következő beszerzése az Oozie-szolgáltatás URL-címe:

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    Ez adatait adja vissza. a következő XML-hasonló:

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    A `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` részére az URL-CÍMÉT az Oozie paranccsal.

2. Használja a következő környezeti változó létrehozása az URL-cím, így nem kell minden parancs típusról:

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    Cserélje le a korábban kapott egy URL-CÍMÉT.
3. A feladat elküldéséhez használja a következő:

    ```
    oozie job -config job.xml -submit
    ```

    Ez a parancs betölti a feladat adatait **job.xml** , és elküldi Oozie, de nem futtatja a programot.

    A parancs után ez a feladat Azonosítóját kell visszaadnia. Például: `0000005-150622124850154-oozie-oozi-W`. Ezt az Azonosítót a feladat kezelésére szolgál.

4. A következő parancsot a feladat állapotának megtekintése:

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > Cserélje le `<JOBID>` az előző lépés eredményeképpen visszakapott azonosítóval.

    Ez hasonló ad vissza adatokat a következő szöveget:

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

    Ez a feladat állapota `PREP`. Ez az állapot azt jelzi, hogy a feladat lett létrehozva, de nem lett elindítva.

5. Az alábbi parancs segítségével indítsa el a feladatot:

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > Cserélje le `<JOBID>` korábban visszaadott azonosítóval.

    Ez a parancs után ellenőrizze az állapot, ha egy futó állapotban van, és információk jelennek-e a feladat a műveletekhez.

6. A feladat sikeres befejeződése után ellenőrizheti, hogy az adatok jön létre, és az SQL-adatbázis táblája exportált a következő parancsokkal:

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    : A `1>` kéri, adja meg a következő lekérdezést:

    ```
    SELECT * FROM mobiledata
    GO
    ```

    A visszakapott információk hasonlít a következő szöveget:

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

A Oozie parancs további információkért lásd: [Oozie parancssori eszköz](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).

## <a name="oozie-rest-api"></a>Oozie REST API-n

Az Oozie REST API lehetővé teszi a saját buildet Oozie együtt használható. HDInsight Oozie REST API használatával kapcsolatos információk a következők:

* **URI**: A REST API érhetők el, a fürtön kívüli`https://CLUSTERNAME.azurehdinsight.net/oozie`

* **Hitelesítési**: az a fürt HTTP-fiókkal (rendszergazda) és a jelszó API hitelesítését. Példa:

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

További információ az Oozie REST API használatával, lásd: [Oozie webszolgáltatási API-ra](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).

## <a name="oozie-web-ui"></a>Oozie webes felhasználói felület

A Oozie webes felhasználói felület révén Oozie feladatok állapotának webes képet a fürtön. A webes felhasználói felület lehetővé teszi a következőket:

* Feladat állapota
* Feladatdefiníció
* Konfiguráció
* A műveletek a feladat grafikonja
* A feladat naplók

Műveletek a feladat részleteit is megtekintheti.

Szeretne használni az Oozie webes felhasználói felületén, tegye a következőket:

1. A HDInsight-fürthöz SSH-alagút létrehozása. További információ: a [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.

2. Ha alagút létrejött, nyissa meg az Ambari webes felhasználói felület a böngészőben. Az URI-azonosítója az Ambari hely **https://CLUSTERNAME.azurehdinsight.net**. Cserélje le **CLUSTERNAME** nevű, a Linux-alapú HDInsight-fürthöz.

3. Válassza ki a lap bal oldalán, **Oozie**, majd **Gyorshivatkozások**, és végül **Oozie webes felhasználói felületén**.

    ![a menük képe](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. Az Oozie webes felhasználói felületének alapértelmezett futó munkafolyamat-feladatok megjelenítése. Minden munkafolyamat-feladatok megtekintéséhez válasszon **összes feladat**.

    ![Minden feladat jelenik meg](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. Jelölje ki a feladatot a feladattal kapcsolatos további információk megjelenítéséhez.

    ![Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. A Feladatinformáció lapján tekintheti meg alapvető feladatinformációkat és az egyes műveletek a feladatban található. Használja a füleket felső megtekintheti a feladatdefiníció, feladat konfigurációs hozzáférés a Job Log vagy egy irányított aciklikus diagramhoz (DAG) a feladat megtekintése.

   * **Job Log**: válassza ki a **GetLogs** gombra kattint, hogy a feladat összes naplójának beolvasása, vagy használja a **keresési szűrőt adjon meg** mező naplók szűrése

       ![Feladat-napló](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * **JobDAG**: az adatbázis-elérhetőségi csoport az adatelérési utak venni a munkafolyamaton keresztül egy grafikus áttekintése

       ![Feladat DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. A műveletek egyikének kiválasztásával a **Feladatinformáció** információk a művelet lapon megjelenik. Válassza például a **RunHiveScript** művelet.

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/action.png)

8. Megtekintheti a részleteket a művelet, például egy hivatkozást a **konzol URL-címe**. Ez a hivatkozás segítségével JobTracker tekintse meg a feladathoz.

## <a name="scheduling-jobs"></a>Feladatütemezés

A koordinátor lehetővé teszi, hogy adja meg egy kezdő, záró és feladatok előfordulási gyakoriságát. A munkafolyamat ütemezését megadásához tegye a következőket:

1. Használja a következő nevű fájl létrehozása **coordinator.xml**:

    ```
    nano coordinator.xml
    ```

    A fájl tartalmát a következő XML-kód használata:

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
    > A `${...}` változók futásidőben feladatdefinícióban értékek helyett. A változók az alábbiak:
    >
    > * `${coordFrequency}`: A feladat példányt futtatva között eltelt idő.
    > ** `${coordStart}`: A feladat kezdési időpontja.
    > * `${coordEnd}`: A feladat befejezési időpontja.
    > * `${coordTimezone}`: Koordinátor feladatok vannak rögzített időzónában nem nyári időszámításra (általában képviseli UTC használatával). Ez az időzóna kezeli a "Oozie feldolgozási időzónában."
    > * `${wfPath}`: Az a workflow.xml elérési útja.

2. Mentse a fájlt, használja a Ctrl-X, **Y**, és **Enter**.

3. Az alábbi parancs segítségével másolja a fájlt a feladat munkakönyvtára:

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. A következő használatával módosíthatja a **job.xml** fájlt:

    ```
    nano job.xml
    ```

    A következő módosításokat:

   * Kérje meg a koordinátor-fájl helyett a munkafolyamat futtatásához oozie, módosítsa `<name>oozie.wf.application.path</name>` való `<name>oozie.coord.application.path</name>`.

   * Beállítása a `workflowPath` használják a koordinátor adja hozzá a következő XML változó:

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       Cserélje le a `wasb://mycontainer@mystorageaccount.blob.core.windows` más bejegyzések a job.xml fájlban megadott értéknek szöveget.

   * A start, végfelhasználók és a koordinátor gyakorisága megadásához adja hozzá a következő XML:

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

       Ezek az értékek értékre van állítva a kezdési időpont 12:00 PM előfordulhat, hogy 10, 2017, 2017. május 12. a befejezési idő. Ez a feladat futtatása naponta intervallumát. A gyakoriság értéke (percben), így 24 óra x 60 perc = 1440 perc. Végezetül UTC időzóna van beállítva.

5. Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.

6. A feladat futtatásához a következő paranccsal:

    ```
    oozie job -config job.xml -run
    ```

    Ez a parancs küldi el, és elindítja a feladatot.

7. Ha látogasson el a Oozie webes felhasználói Felületet, és válassza a **koordinátor feladatok** lapon megjelenik az alábbi képen hasonló adatokat:

    ![koordinátor feladatok lap](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    A **következő Materialization** -bejegyzése tartalmazza a feladat fut. a következő újraindításkor.

8. Hasonló a korábbi munkafolyamat-feladatot, válassza ki a projekt tételt a webes felhasználói felület megjeleníti az információkat a feladat:

    ![Koordinátor Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > Ez a rendszerkép csak a feladat sikeres futtatása az ütemezett munkafolyamaton belül nem az egyéni műveletek jeleníti meg. Amely megtekintéséhez válasszon a **művelet** bejegyzéseket.

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a>Hibaelhárítás

A Oozie felhasználói felület lehetővé teszi Oozie naplókat. A munkafolyamat által elindított MapReduce-feladatok JobTracker naplókat mutató hivatkozásokat is tartalmaz. A minta hibaelhárítási kell lennie:

1. Tekintse meg a feladatot Oozie webes felhasználói felületén.

2. Ha egy hiba vagy egy adott művelet sikertelen, válassza ki a művelet, ha a **hibaüzenet** mező további információkat tartalmazza a hiba esetén.

3. Ha elérhető, a művelet az URL-CÍMÉT segítségével további részleteket (például a Hadoopból naplók) a művelet.

A következő bizonyos hibákat tapasztalhat, és azok megoldását.

### <a name="ja009-cannot-initialize-cluster"></a>JA009: Nem lehet inicializálni a fürt

**A jelenség**: A feladat állapota **FELFÜGGESZTETT**. A feladat részleteinek megjelenítése a RunHiveScript állapotának **START_MANUAL**. A művelet kiválasztása a következő hibaüzenetet jeleníti meg:

    JA009: Cannot initialize Cluster. Please check your configuration for map

**OK**: A WASB használt címek a **job.xml** fájl nem tartalmaz, a tároló vagy a tárfiók nevét. A WASB cím formátumban kell megadni `wasb://containername@storageaccountname.blob.core.windows.net`.

**Megoldási**: módosítsa a WASB címeket, a feladat használja.

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a>JA002: Oozie nem lehet megszemélyesíteni &lt;felhasználó >

**A jelenség**: A feladat állapota **FELFÜGGESZTETT**. A feladat részleteinek megjelenítése a RunHiveScript állapotának **START_MANUAL**. A művelet kiválasztása jeleníti meg a következő hibaüzenet:

    JA002: User: oozie is not allowed to impersonate <USER>

**OK**: jelenlegi engedélybeállítások nem teszik lehetővé az Oozie megszemélyesíthet-e a megadott felhasználói fiók.

**Megoldási**: Oozie megszemélyesíteni a felhasználók számára engedélyezett a **felhasználók** csoport. Használja a `groups USERNAME` megtekintéséhez a csoportokat, amelyek a felhasználói fiók tagja. Ha a felhasználó nem tagja a **felhasználók** csoportjában vegye fel a felhasználót a csoportba a következő paranccsal:

    sudo adduser USERNAME users

> [!NOTE]
> HDInsight felismeri, hogy a felhasználó hozzáadta-e a csoport több percig is eltarthat.

### <a name="launcher-error-sqoop"></a>Hiba (Sqoop) indítója

**A jelenség**: A feladat állapota **KILLED**. A feladat részleteinek megjelenítése a RunSqoopExport állapotának **hiba**. A művelet kiválasztása jeleníti meg a következő hibaüzenet:

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

**OK**: Sqoop nem tudja betölteni az adatbázis eléréséhez szükséges adatbázis-illesztőprogramját.

**Megoldási**: amikor egy Oozie-feladat a Sqoop használ, meg kell adni a feladat által használt adatbázis illesztőprogram más erőforrások (például a workflow.xml). Az adatbázis-illesztőprogramját tartalmazó archív is hivatkozhat a `<sqoop>...</sqoop>` a workflow.xml szakasza.

Például a feladat ebben a dokumentumban, akkor használja az alábbi lépéseket:

1. Másolja a sqljdbc4.1.jar fájlt a /tutorials/useoozie könyvtár:

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. Módosítsa a következő XML-kód hozzáadása a fenti új sor workflow.xml `</sqoop>`:

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban megtanulta, hogyan adhat meg egy Oozie munkafolyamat és az Oozie-feladat futtatása. A HDInsight használata kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:

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
