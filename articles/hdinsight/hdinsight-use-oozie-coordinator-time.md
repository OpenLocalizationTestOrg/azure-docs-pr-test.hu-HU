---
title: "aaaUse a hdinsight meg időalapú Hadoop Oozie a koordinátor |} Microsoft Docs"
description: "Időalapú Hadoop Oozie-koordinátor használata a Hdinsightban, big data-szolgáltatása. Ismerje meg, hogyan toodefine Oozie munkafolyamatok és a koordinátor, valamint feladatok elküldéséhez."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00c3a395-d51a-44ff-af2d-1f116c4b1c83
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: aecbb5ee94a4234d1a7768bdb6de2a33508b1e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-time-based-oozie-coordinator-with-hadoop-in-hdinsight-toodefine-workflows-and-coordinate-jobs"></a>Hadoop HDInsight toodefine munkafolyamatok időalapú Oozie-koordinátor használata, és összehangolják a feladatok
Ebből a cikkből megtudhatja, hogyan toodefine munkafolyamatok és a koordinátor, és hogyan tootrigger hello koordinátor-feladatokhoz, ideje alapján. Hasznos toogo keresztül [és a HDInsight együttes használata Oozie] [ hdinsight-use-oozie] Ez a cikk olvasása előtt. Ezenkívül tooOozie, is ütemezheti az Azure Data Factory használatával feladatok. Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive](../data-factory/data-factory-data-transformation-activities.md).

> [!NOTE]
> Ez a cikk egy Windows-alapú HDInsight-fürt szükséges. Oozie, többek között a feladatok időalapú egy Linux-alapú fürtön használatáról lásd: [használata Oozie Hadoop toodefine és a Linux-alapú HDInsight a munkafolyamat futtatása](hdinsight-use-oozie-linux-mac.md)

## <a name="what-is-oozie"></a>Mi az az Oozie
Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli. Hadoop-veremmel hello integrálva van, és támogatja a Hadoop-feladatokat Apache MapReduce, Apache Pig, Apache Hive és Apache Sqoop. Azt is, amelyek adott tooa rendszerén, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok.

hello következő kép bemutatja hello munkafolyamat fogja végrehajtani:

![Munkafolyamat diagramja][img-workflow-diagram]

hello munkafolyamat két műveletet tartalmaz:

1. A Hive művelet a futás a HiveQL parancsfájl toocount hello napló szintű típusonkénti előfordulások log4j naplófájlban. Minden egyes log4j napló például a [NAPLÓZÁSI szint] mező tooshow hello típusát és hello súlyossága, tartalmazó sor mezők áll:

        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...

    hello Hive parancsfájl kimenetében hasonlít:

        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4

    További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].
2. A Sqoop művelet Azure SQL adatbázis hello tooa HiveQL műveleti eredménytábla exportálja. További információ a Sqoop: [és a HDInsight együttes használata Sqoop][hdinsight-use-sqoop].

> [!NOTE]
> Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello fürt verziók?] [hdinsight-versions].
>
>

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez hello következő kell rendelkeznie:

* **Munkaállomás Azure PowerShell-lel**.

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnik. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Kövesse a lépéseket hello [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb Azure PowerShell verziója. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md) további információt.

* **HDInsight-fürtök**. HDInsight-fürtök létrehozásával kapcsolatos további információkért lásd: [HDInsight-fürtök létrehozása][hdinsight-provision], vagy [első lépései a hdinsight eszközzel][hdinsight-get-started]. A következő adatok toogo hello oktatóanyag hello lesz szüksége:

    <table border = "1">
    <tr><th>Fürt tulajdonság</th><th>A Windows PowerShell-változó neve</th><th>Érték</th><th>Leírás</th></tr>
    <tr><td>A HDInsight-fürt neve</td><td>$clusterName</td><td></td><td>hello HDInsight fürt, amelyen futtatja az oktatóanyag.</td></tr>
    <tr><td>A HDInsight fürt felhasználónév</td><td>$clusterUsername</td><td></td><td>hello HDInsight fürt felhasználó felhasználóneve. </td></tr>
    <tr><td>A HDInsight fürt felhasználói jelszó </td><td>$clusterPassword</td><td></td><td>hello HDInsight fürt felhasználói jelszavát.</td></tr>
    <tr><td>Az Azure storage-fiók neve</td><td>$storageAccountName</td><td></td><td>Azure Storage fiók elérhető toohello HDInsight-fürtöt. Ebben az oktatóanyagban hello alapértelmezett tárfiókot, hello fürt kiépítési folyamat során adott meg használni.</td></tr>
    <tr><td>Az Azure Blob-tároló neve</td><td>$containerName</td><td></td><td>Ebben a példában hello Azure Blob storage tárolók használt hello alapértelmezett HDInsight-fürt fájlrendszert használja. Alapértelmezés szerint hello azonos nevet hello HDInsight-fürttel rendelkezik.</td></tr>
    </table>
* **Azure SQL-adatbázis**. Konfigurálnia kell egy tűzfalszabályt hello SQL adatbázis-kiszolgáló tooallow hozzáférés a munkaállomáson. Egy Azure SQL-adatbázis létrehozása, és hello tűzfal konfigurálásával kapcsolatos útmutatásért lásd: [használatának első Azure SQL adatbázis] [sqldatabase-get-started]. A cikkben egy Windows PowerShell-parancsfájl létrehozásához hello Azure SQL-adatbázistáblában szereplő, amelyekre szüksége van ehhez az oktatóanyaghoz.

    <table border = "1">
    <tr><th>SQL-adatbázis tulajdonság</th><th>A Windows PowerShell-változó neve</th><th>Érték</th><th>Leírás</th></tr>
    <tr><td>SQL adatbázis-kiszolgáló neve</td><td>$sqlDatabaseServer</td><td></td><td>hello SQL database kiszolgáló toowhich Sqoop exportálja az adatokat. </td></tr>
    <tr><td>SQL adatbázis-bejelentkezési név</td><td>$sqlDatabaseLogin</td><td></td><td>SQL adatbázis-bejelentkezési név.</td></tr>
    <tr><td>SQL-adatbázis a bejelentkezés jelszó</td><td>$sqlDatabaseLoginPassword</td><td></td><td>SQL-adatbázis bejelentkezési jelszót.</td></tr>
    <tr><td>SQL-adatbázis neve</td><td>$sqlDatabaseName</td><td></td><td>hello Azure SQL adatbázis toowhich Sqoop exportálja az adatokat. </td></tr>
    </table>

  > [!NOTE]
  > Alapértelmezés szerint az Azure SQL database az Azure-szolgáltatások, például az Azure HDInsight engedélyezi a csatlakozást. Ha a tűzfal beállítás nincs engedélyezve, engedélyeznie kell azt a hello Azure portálon. Az utasítás SQL-adatbázis létrehozása és a tűzfal-szabályok konfigurálása, lásd: [létrehozása és konfigurálása az SQL-adatbázis][sqldatabase-get-started].

> [!NOTE]
> Kitöltési hello értékek hello táblában. Hasznos lehet az oktatóanyag lépéseinek lesz.

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a>Adja meg az Oozie munkafolyamat és a hello kapcsolódó HiveQL-parancsfájlt
Oozie munkafolyamatok definíciók hPDL (az XML folyamat definition language) nyelven íródtak. hello alapértelmezett munkafolyamat Fájlnév *workflow.xml*.  Mentse helyileg a hello munkafolyamat fájlt fog, és majd telepítenie kell azt toohello HDInsight-fürt Azure PowerShell használatával az oktatóanyag későbbi részében.

hello Hive művelet hello munkafolyamat meghívja a HiveQL-parancsfájlt. A parancsfájl három HiveQL utasításokat tartalmazza:

1. **hello DROP TABLE utasítás** törlések hello log4j Hive táblát, ha van ilyen.
2. **CREATE TABLE utasítás hello** log4j Hive külső tábla létrehozása, amely hello log4j naplófájl; toohello helyre mutat
3. **hello log4j naplófájl helye hello**. hello mező határoló ",". hello alapértelmezett sor elválasztó karaktere "\n". A Hive külső tábla használt tooavoid hello adatfájl távolít el hello eredeti helyére,, abban az esetben, ha azt szeretné toorun hello Oozie munkafolyamat több alkalommal.
4. **hello FELÜLÍRÁSA INSERT utasítás** a napló szintű típusonkénti hello előfordulások száma hello log4j Hive táblát, és hello kimeneti tooan Azure Blob storage helyre menti.

> [!NOTE]
> Nincs olyan ismert Hive útvonallal kapcsolatos probléma. Ha egy Oozie feladat elküldése elindul ezt a problémát. hello utasításokat a hello a probléma kijavítása találhatók a TechNet Wiki hello: [HDInsight Hive-hiba: nem toorename][technetwiki-hive-error].

**toodefine hello HiveQL parancsfájl fájl toobe hello munkafolyamat által meghívott**

1. Hozzon létre egy szövegfájlt az alábbi hello tartalom:

        DROP TABLE ${hiveTableName};
        CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
        INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

    Három változók hello parancsfájl használatban van:

   * ${hiveTableName}
   * ${hiveDataFolder}
   * ${hiveOutputFolder}

     hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) futási időben ezen értékek toothis HiveQL-parancsfájlt sikeres lesz.
2. Hello fájl mentése másként **C:\Tutorials\UseOozie\useooziewf.hql** ANSI (ASCII) kódolással. (Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.) A parancsfájl telepített toohello HDInsight-fürt lesz hello oktatóanyag későbbi részében.

**egy munkafolyamat toodefine**

1. Hozzon létre egy szövegfájlt az alábbi hello tartalom:

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
                <param>hiveOutputFolder=${hiveOutputFolder}</param>
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
            <arg>${hiveOutputFolder}</arg>
            <arg>-m</arg>
            <arg>1</arg>
            <arg>--input-fields-terminated-by</arg>
            <arg>"\001"</arg>
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

    Nincsenek definiált hello munkafolyamat két műveleteket. hello start-tooaction van *RunHiveScript*. Ha hello művelet a futás *OK*, hello a következő művelet *RunSqoopExport*.

    hello RunHiveScript több változót tartalmaz. Hello értékeket fogja továbbítani, amikor hello Oozie feladat munkaállomáson Azure PowerShell használatával.

    Munkafolyamat-változók

    <table border = "1">
    <tr><th>Munkafolyamat-változók</th><th>Leírás</th></tr>
    <tr><td>${jobTracker}</td><td>Adja meg a Hadoop-feladat követő hello hello URL-CÍMÉT. Használjon <strong>jobtrackerhost:9010</strong> a HDInsight fürt 3.0-s és 2.0-s verzióját.</td></tr>
    <tr><td>${nameNode}</td><td>Adja meg a hello Hadoop neve csomópont hello URL-CÍMÉT. Használja a hello alapértelmezett fájl rendszer wasb: / / címmel, például <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</td></tr>
    <tr><td>${queueName}</td><td>Adja meg a feladat hello hello várólistacímke elküldve. Használjon <strong>alapértelmezett</strong>.</td></tr>
    </table>

    Hive művelet változói

    <table border = "1">
    <tr><th>Műveleti változó struktúra</th><th>Leírás</th></tr>
    <tr><td>${hiveDataFolder}</td><td>Hive Create Table parancs hello hello forráskönyvtárat.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>kimeneti mappa hello hello FELÜLÍRÁSA INSERT utasítás.</td></tr>
    <tr><td>${hiveTableName}</td><td>hello Hive tábla által hivatkozott hello log4j adatfájlok hello neve.</td></tr>
    </table>

    Sqoop művelet változói

    <table border = "1">
    <tr><th>Sqoop művelet változó</th><th>Leírás</th></tr>
    <tr><td>${sqlDatabaseConnectionString}</td><td>SQL adatbázis-kapcsolati karakterlánc.</td></tr>
    <tr><td>${sqlDatabaseTableName}</td><td>hello Azure SQL adatbázis toowhere hello adatait exportálja.</td></tr>
    <tr><td>${hiveOutputFolder}</td><td>a Hive BESZÚRÁSA FELÜLÍRÁSA utasítás hello hello a kimeneti mappa. Ez a hello ugyanabban a mappában hello Sqoop export (Exportálás-dir).</td></tr>
    </table>

    Oozie munkafolyamat és a munkafolyamat-műveleteket hello használatával kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).

1. Hello fájl mentése másként **C:\Tutorials\UseOozie\workflow.xml** ANSI (ASCII) kódolással. (Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)

**toodefine koordinátora**

1. Hozzon létre egy szövegfájlt az alábbi hello tartalom:

    ```xml
    <coordinator-app name="my_coord_app" frequency="${coordFrequency}" start="${coordStart}" end="${coordEnd}" timezone="${coordTimezone}" xmlns="uri:oozie:coordinator:0.4">
        <action>
            <workflow>
                <app-path>${wfPath}</app-path>
            </workflow>
        </action>
    </coordinator-app>
    ```

    Öt változók hello csomagdefiníciós fájl használatban van:

   | Változó | Leírás |
   | --- | --- |
   | ${coordFrequency} |A feladat felfüggesztése időpontja. Gyakoriság mindig kifejezett perc múlva. |
   | ${coordStart} |A feladat kezdési időpontja. |
   | ${coordEnd} |A feladat befejezési időpontja. |
   | ${coordTimezone} |Oozie coordinator feladatok rögzített időzónában dolgozza nem nyári időszámítás (általában képviseli UTC használatával). Ez az időzóna kezeli hello "Oozie feldolgozási időzóna." |
   | ${wfPath} |hello workflow.xml hello elérési útját.  Ha hello munkafolyamat fájlnév nem hello alapértelmezett neve (workflow.xml), meg kell adnia. |
2. Hello fájl mentése másként **C:\Tutorials\UseOozie\coordinator.xml** hello ANSI (ASCII) kódolással. (Használja a Jegyzettömböt, ha a szövegszerkesztőben nem adja meg ezt a lehetőséget.)

## <a name="deploy-hello-oozie-project-and-prepare-hello-tutorial"></a>Hello Oozie projekt telepítése és előkészítése hello oktatóanyag
Az Azure PowerShell parancsfájl tooperform hello következő futtatja:

* Másolás hello HiveQL parancsfájl (useoozie.hql) tooAzure Blob-tároló, wasb:///tutorials/useoozie/useoozie.hql.
* Workflow.xml toowasb:///tutorials/useoozie/workflow.xml másolja.
* Coordinator.xml toowasb:///tutorials/useoozie/coordinator.xml másolja.
* Hello adatok fájl másolása (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.
* Hozzon létre egy Azure SQL-adatbázistáblában szereplő Sqoop exportálási adatainak tárolásához. hello táblázat neve *log4jLogCount*.

**HDInsight tároló ismertetése**

HDInsight Azure Blob storage-tárolására használ. wasb: / / hello Hadoop elosztott fájlrendszer (HDFS) az Azure Blob storage a Microsoft általi implementációja. További információkért lásd: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].

HDInsight-fürtök kiépítése, amikor egy Azure Blob storage-fiókot és egy adott tárolóhoz ebből a fiókból kijelölt hello alapértelmezett fájlrendszerben, például a HDFS-ben. Ezenkívül toothis tárfiókot, hozzáadhat további tárfiókok a hello azonos Azure-előfizetés vagy az Azure-előfizetések hello kiépítési folyamat során. További tárfiókok hozzáadásáról útmutatásért lásd: [Provision HDInsight clusters][hdinsight-provision]. toosimplify hello Azure PowerShell-parancsprogram ebben az oktatóanyagban használt összes fájljai hello alapértelmezett fájl rendszertárolóhoz hello helyen */oktatóanyagok/useoozie*. Alapértelmezés szerint ebben a tárolóban van hello azonos nevet hello HDInsight fürt neveként.
hello szintaxisa:

    wasb[s]://<ContainerName>@<StorageAccountName>.blob.core.windows.net/<path>/<filename>

> [!NOTE]
> Csak hello *wasb: / /* szintaxis a HDInsight fürt 3.0-s verziója támogatott. hello régebbi *asv: / /* HDInsight 2.1-es és 1.6 fürtök a szintaxis támogatott, de nem támogatott a HDInsight 3.0 fürtök.
>
> hello wasb: / / elérési út egy virtuális elérési út. További információ: [használata Azure Blob storage a hdinsight eszközzel][hdinsight-storage].

Hello alapértelmezett fájl rendszer tárolóban tárolt fájlt érhetők el a HDInsight-ból a következő URI-azonosítók (használok workflow.xml példaként) hello egyikével sem:

    wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie/workflow.xml
    wasb:///tutorials/useoozie/workflow.xml
    /tutorials/useoozie/workflow.xml

Ha azt szeretné, hogy közvetlenül a hello tárfiók tooaccess hello fájl, hello blob neve hello fájl van:

    tutorials/useoozie/workflow.xml

**Hive belső és külső táblák ismertetése**

Néhány dolgot Hive belső és külső táblák tooknow van szüksége:

* hello CREATE TABLE parancs létrehoz egy belső tábla, más néven a felügyelt. hello adatfájl hello alapértelmezett tárolóban kell lennie.
* CREATE TABLE parancs hello áthelyezi hello adatok toohello fájl, / / adatraktár/hive<TableName> hello alapértelmezett tároló mappa.
* hello külső tábla létrehozása parancs létrehoz egy külső táblát. hello adatfájl hello alapértelmezett tároló kívül is kell elhelyezni.
* a külső tábla létrehozása parancs hello nem helyezi át a hello adatfájlt.
* hello külső tábla létrehozása parancs almappákban hello hely záradékban megadott hello mappában nem engedélyezi. Ez oka hello miért hello oktatóanyag hello sample.log fájl másolatot készít.

További információkért lásd: [HDInsight: Hive belső és külső táblák bevezetés][cindygross-hive-tables].

**tooprepare hello oktatóanyag**

1. Nyissa meg hello Windows PowerShell ISE (hello Windows 8 kezdőképernyőn írja be **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**. További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).
2. Hello alsó ablaktáblában futtassa a következő parancs tooconnect tooyour Azure-előfizetés hello:

    ```powershell
    Add-AzureAccount
    ```

    Akkor lesz felszólító tooenter a Azure-fiók hitelesítő adatait. Ez a módszer az előfizetés-kapcsolat hozzáadása túllépi az időkorlátot, és 12 óra elteltével fog toorun hello parancsmag újra.

   > [!NOTE]
   > Ha több Azure-előfizetéssel rendelkezik, és hello alapértelmezett előfizetés nincs hello kívánt toouse, használja a hello <strong>válasszon-AzureSubscription</strong> parancsmag tooselect előfizetés.

3. Másolja a következő parancsfájl hello parancsfájl ablaktáblára hello, és utána állítsa be a hello első hat változók:

    ```powershell
    # WASB variables
    $storageAccountName = "<StorageAccountName>"
    $containerName = "<BlobStorageContainerName>"

    # SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    # Oozie files for hello tutorial
    $hiveQLScript = "C:\Tutorials\UseOozie\useooziewf.hql"
    $workflowDefinition = "C:\Tutorials\UseOozie\workflow.xml"
    $coordDefinition =  "C:\Tutorials\UseOozie\coordinator.xml"

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here
    ```

    Hello változók további leírását lásd: hello [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.

4. A következő toohello parancsfájl hello parancsfájl ablaktáblán hello hozzáfűzése:

    ```powershell
    # Create a storage context object
    $storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey

    function uploadOozieFiles()
    {
        Write-Host "Copy HiveQL script, workflow definition and coordinator definition ..." -ForegroundColor Green
        Set-AzureStorageBlobContent -File $hiveQLScript -Container $containerName -Blob "$destFolder/useooziewf.hql" -Context $destContext
        Set-AzureStorageBlobContent -File $workflowDefinition -Container $containerName -Blob "$destFolder/workflow.xml" -Context $destContext
        Set-AzureStorageBlobContent -File $coordDefinition -Container $containerName -Blob "$destFolder/coordinator.xml" -Context $destContext
    }

    function prepareHiveDataFile()
    {
        Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green
        Start-CopyAzureStorageBlob -SrcContainer $containerName -SrcBlob "example/data/sample.log" -Context $destContext -DestContainer $containerName -destBlob "$destFolder/data/sample.log" -DestContext $destContext
    }

    function prepareSQLDatabase()
    {
        # SQL query string for creating log4jLogsCount table
        $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
                [Level] [nvarchar](10) NOT NULL,
                [Total] float,
            CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
            (
            [Level] ASC
            )
            )"

        #Create hello log4jLogsCount table
        Write-Host "Create Log4jLogsCount table ..." -ForegroundColor Green
        $conn = New-Object System.Data.SqlClient.SqlConnection
        $conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
        $conn.open()
        $cmd = New-Object System.Data.SqlClient.SqlCommand
        $cmd.connection = $conn
        $cmd.commandtext = $cmdCreateLog4jCountTable
        $cmd.executenonquery()

        $conn.close()
    }

    # upload workflow.xml, coordinator.xml, and ooziewf.hql
    uploadOozieFiles;

    # make a copy of example/data/sample.log tooexample/data/log4j/sample.log
    prepareHiveDataFile;

    # create log4jlogsCount table on SQL database
    prepareSQLDatabase;
    ```

5. Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl. hello kimeneti hasonló lesz:

    ![Útmutató előkészítési kimeneti][img-preparation-output]

## <a name="run-hello-oozie-project"></a>Hello Oozie-projekt futtatása
Az Azure PowerShell jelenleg nem biztosít semmilyen parancsmagok Oozie feladatok meghatározásához. Használhatja a hello **Invoke-RestMethod** parancsmag tooinvoke Oozie webszolgáltatások. hello Oozie webszolgáltatások API egy HTTP REST API-t JSON. Hello Oozie webszolgáltatások API kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight-fürt verziószáma 3.0) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight-fürt verziószáma 2.1-es).

**toosubmit egy Oozie feladat**

1. Nyissa meg hello Windows PowerShell ISE (a Windows 8 kezdőképernyőn írja be **PowerShell_ISE**, és kattintson a **Windows PowerShell ISE**. További információkért lásd: [Windows PowerShell elindítása a Windows 8 és Windows][powershell-start]).
2. Másolás hello alábbi parancsfájl hello parancsfájl ablaktáblára, és majd a készlet első 14 változók hello (azonban kihagyása **$storageUri**).

    ```powershell
    #HDInsight cluster variables
    $clusterName = "<HDInsightClusterName>"
    $clusterUsername = "<HDInsightClusterUsername>"
    $clusterPassword = "<HDInsightClusterUserPassword>"

    #Azure Blob storage (WASB) variables
    $storageAccountName = "<StorageAccountName>"
    $storageContainerName = "<BlobContainerName>"
    $storageUri="wasb://$storageContainerName@$storageAccountName.blob.core.windows.net"

    #Azure SQL database variables
    $sqlDatabaseServer = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabaseLoginPassword = "<SQLDatabaseloginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"

    #Oozie WF/coordinator variables
    $coordStart = "2014-03-21T13:45Z"
    $coordEnd = "2014-03-21T13:45Z"
    $coordFrequency = "1440"    # in minutes, 24h x 60m = 1440m
    $coordTimezone = "UTC"    #UTC/GMT

    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServer.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServer;password=$sqlDatabaseLoginPassword;database=$sqlDatabaseName"
    $sqlDatabaseTableName = "log4jLogsCount"

    $passwd = ConvertTo-SecureString $clusterPassword -AsPlainText -Force
    $creds = New-Object System.Management.Automation.PSCredential ($clusterUsername, $passwd)
    ```

    Hello változók további leírását lásd: hello [Előfeltételek](#prerequisites) ebben az oktatóanyagban szakasz.

    $coordstart és $coordend hello munkafolyamat kezdési és befejezési idő. toofind kimenő hello UTC/GMT szerinti idő, keressen a "UTC szerinti idő" bing.com. hello $coordFrequency milyen gyakran toorun hello munkafolyamat percben van.
3. A következő toohello parancsfájl hello hozzáfűzése. Meghatározza, hogy ez a kijelző hello Oozie hasznos:

    ```powershell
    #OoziePayload used for Oozie web service submission
    $OoziePayload =  @"
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>

        <property>
            <name>nameNode</name>
            <value>$storageUrI</value>
        </property>

        <property>
            <name>jobTracker</name>
            <value>jobtrackerhost:9010</value>
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
            <name>oozie.coord.application.path</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>wfPath</name>
            <value>$oozieWFPath</value>
        </property>

        <property>
            <name>coordStart</name>
            <value>$coordStart</value>
        </property>

        <property>
            <name>coordEnd</name>
            <value>$coordEnd</value>
        </property>

        <property>
            <name>coordFrequency</name>
            <value>$coordFrequency</value>
        </property>

        <property>
            <name>coordTimezone</name>
            <value>$coordTimezone</value>
        </property>

        <property>
            <name>hiveScript</name>
            <value>$hiveScript</value>
        </property>

        <property>
            <name>hiveTableName</name>
            <value>$hiveTableName</value>
        </property>

        <property>
            <name>hiveDataFolder</name>
            <value>$hiveDataFolder</value>
        </property>

        <property>
            <name>hiveOutputFolder</name>
            <value>$hiveOutputFolder</value>
        </property>

        <property>
            <name>sqlDatabaseConnectionString</name>
            <value>&quot;$sqlDatabaseConnectionString&quot;</value>
        </property>

        <property>
            <name>sqlDatabaseTableName</name>
            <value>$SQLDatabaseTableName</value>
        </property>

        <property>
            <name>user.name</name>
            <value>admin</value>
        </property>

    </configuration>
    "@
    ```

   > [!NOTE]
   > hello képest jelentős különbség toohello munkafolyamat küldésének hasznos fájl hello változó **oozie.coord.application.path**. Amikor egy munkafolyamat-feladat elküldéséhez használhatja **oozie.wf.application.path** helyette.

4. A következő toohello parancsfájl hello hozzáfűzése. Ez a kijelző hello Oozie webes szolgáltatás állapotát ellenőrzi:

    ```powershell
    function checkOozieServerStatus()
    {
        Write-Host "Checking Oozie server status..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/admin/status"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds -OutVariable $OozieServerStatus

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieServerSatus = $jsonResponse[0].("systemMode")
        Write-Host "Oozie server status is $oozieServerSatus..."

        if($oozieServerSatus -notmatch "NORMAL")
        {
            Write-Host "Oozie server status is $oozieServerSatus...cannot submit Oozie jobs. Check hello server status and re-run hello job."
            exit 1
        }
    }
    ```

5. A következő toohello parancsfájl hello hozzáfűzése. Ez a kijelző egy Oozie feladat létrehozása:

    ```powershell
    function createOozieJob()
    {
        # create Oozie job
        Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
        Write-Host "`n--------`n$OoziePayload`n--------"
        $clusterUriCreateJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $creds -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName -debug -Verbose

        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $oozieJobId = $jsonResponse[0].("id")
        Write-Host "Oozie job id is $oozieJobId..."

        return $oozieJobId
    }
    ```

   > [!NOTE]
   > Amikor egy munkafolyamat-feladat, meg kell nyitnia egy másik webes szolgáltatás hívása toostart hello feladat hello feladat létrehozása után. Ebben az esetben hello koordinátor feladat idő váltja ki. hello feladat automatikusan elindul.

6. A következő toohello parancsfájl hello hozzáfűzése. Ez a kijelző hello Oozie feladat állapotát ellenőrzi:

    ```powershell
    function checkOozieJobStatus($oozieJobId)
    {
        # get job status
        Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

        Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
        $clusterUriGetJobStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")

        while($JobStatus -notmatch "SUCCEEDED|KILLED")
        {
            Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
            Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
            $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $creds
            $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
            $JobStatus = $jsonResponse[0].("status")
        }

        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!"
        if($JobStatus -notmatch "SUCCEEDED")
        {
            Write-Host "Check logs at http://headnode0:9014/cluster for detais."
            exit -1
        }
    }
    ```

7. (Választható) A következő toohello parancsfájl hello hozzáfűzése.

    ```powershell
    function listOozieJobs()
    {
        Write-Host "Listing Oozie jobs..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/jobs"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds

        write-host "Job ID                                   App Name        Status      Started                         Ended"
        write-host "----------------------------------------------------------------------------------------------------------------------------------"
        foreach($job in $response.workflows)
        {
            Write-Host $job.id "`t" $job.appName "`t" $job.status "`t" $job.startTime "`t" $job.endTime
        }
    }

    function ShowOozieJobLog($oozieJobId)
    {
        Write-Host "Showing Oozie job info..." -ForegroundColor Green
        $clusterUriStatus = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/$oozieJobId" + "?show=log"
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $creds
        write-host $response
    }

    function killOozieJob($oozieJobId)
    {
        Write-Host "Killing hello Oozie job $oozieJobId..." -ForegroundColor Green
        $clusterUriStartJob = "https://$clusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=kill" #Valid values for hello 'action' parameter are 'start', 'suspend', 'resume', 'kill', 'dryrun', 'rerun', and 'change'.
        $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $creds | Format-Table -HideTableHeaders -debug
    }
    ```

8. A következő toohello parancsfájl hello hozzáfűzése:

    ```powershell
    checkOozieServerStatus
    # listOozieJobs
    $oozieJobId = createOozieJob($oozieJobId)
    checkOozieJobStatus($oozieJobId)
    # ShowOozieJobLog($oozieJobId)
    # killOozieJob($oozieJobId)
    ```

    Távolítsa el a hello # jelet, ha azt szeretné, hogy toorun hello további funkciók.
9. Ha a HDinsight-fürt 2.1-es verziója, cserélje le a "https://$clusterName.azurehdinsight.net:443/oozie/v2/" a "https://$clusterName.azurehdinsight.net:443/oozie/v1/". HDInsight-fürt verziószáma 2.1 nem támogatja a 2-es verziójának hello webszolgáltatások végzi.
10. Kattintson a **-parancsfájl futtatása** vagy nyomja le az ENTER **F5** toorun hello parancsfájl. hello kimeneti hasonló lesz:

     ![Útmutató munkafolyamat kimeneti futtatása][img-runworkflow-output]
11. Csatlakozás SQL Database toosee hello exportált adatok tooyour.

**toocheck hello feladat hibanapló**

egy munkafolyamat tootroubleshoot, hello Oozie naplófájl helyen találhatók C:\apps\dist\oozie-3.3.2.1.3.2.0-05\oozie-win-distro\logs\Oozie.log a hello fürt headnode. Az RDP információkért lásd: [felügyelete HDInsight-fürtök hello Azure-portálon][hdinsight-admin-portal].

**toorerun hello oktatóanyag**

toorerun hello munkafolyamat, végezze el a következő feladatok hello:

* Hello Hive parancsfájl kimeneti fájl törlése.
* Hello log4jLogsCount tábla hello adatok törlése.

Íme egy minta Windows PowerShell-parancsfájl használható:

```powershell
$storageAccountName = "<AzureStorageAccountName>"
$containerName = "<ContainerName>"

#SQL database variables
$sqlDatabaseServer = "<SQLDatabaseServerName>"
$sqlDatabaseLogin = "<SQLDatabaseLoginName>"
$sqlDatabaseLoginPassword = "<SQLDatabaseLoginPassword>"
$sqlDatabaseName = "<SQLDatabaseName>"
$sqlDatabaseTableName = "log4jLogsCount"

Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
$storageaccountkey = get-azurestoragekey $storageAccountName | %{$_.Primary}
$destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageaccountkey
Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $containerName

Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
$conn = New-Object System.Data.SqlClient.SqlConnection
$conn.ConnectionString = "Data Source=$sqlDatabaseServer.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabaseLoginPassword;Encrypt=true;Trusted_Connection=false;"
$conn.open()
$cmd = New-Object System.Data.SqlClient.SqlCommand
$cmd.connection = $conn
$cmd.commandtext = "delete from $sqlDatabaseTableName"
$cmd.executenonquery()

$conn.close()
```

## <a name="next-steps"></a>Következő lépések
Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat- és az Oozie-koordinátor, és hogyan toorun az Oozie-koordinátor feladat Azure PowerShell használatával. toolearn több, tekintse meg a következő cikkek hello:

* [Első lépései a hdinsight eszközzel][hdinsight-get-started]
* [Használhat Azure Blob Storage tárolót a hdinsight eszközzel][hdinsight-storage]
* [HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]
* [Adatok tooHDInsight feltöltése][hdinsight-upload-data]
* [Use Sqoop with HDInsight][hdinsight-use-sqoop]
* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-java-mapreduce]

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563

[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-develop-java-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md

[sqldatabase-get-started]: ../sql-database/sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md

[apache-hadoop]: http://hadoop.apache.org/
[apache-oozie-400]: http://oozie.apache.org/docs/4.0.0/
[apache-oozie-332]: http://oozie.apache.org/docs/3.3.2/

[powershell-download]: http://azure.microsoft.com/downloads/
[powershell-about-profiles]: http://go.microsoft.com/fwlink/?LinkID=113729
[powershell-install-configure]: /powershell/azureps-cmdlets-docs
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx
[powershell-script]: http://technet.microsoft.com/library/ee176949.aspx

[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx

[img-workflow-diagram]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Workflow.Diagram.png
[img-preparation-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.Preparation.Output1.png
[img-runworkflow-output]: ./media/hdinsight-use-oozie-coordinator-time/HDI.UseOozie.RunCoord.Output.png

[technetwiki-hive-error]: http://social.technet.microsoft.com/wiki/contents/articles/23047.hdinsight-hive-error-unable-to-rename.aspx
