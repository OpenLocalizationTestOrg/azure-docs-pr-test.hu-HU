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
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-on-linux-based-hdinsight"></a><span data-ttu-id="8768f-104">Hadoop toodefine Oozie használjon, és a Linux-alapú HDInsight munkafolyamat futtatása</span><span class="sxs-lookup"><span data-stu-id="8768f-104">Use Oozie with Hadoop toodefine and run a workflow on Linux-based HDInsight</span></span>

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="8768f-105">Megtudhatja, hogyan toouse Hadoop on HDInsight az Apache Oozie.</span><span class="sxs-lookup"><span data-stu-id="8768f-105">Learn how toouse Apache Oozie with Hadoop on HDInsight.</span></span> <span data-ttu-id="8768f-106">Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli.</span><span class="sxs-lookup"><span data-stu-id="8768f-106">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="8768f-107">Oozie hello Hadoop-veremmel integrálva van, és a következő feladatok hello támogatja:</span><span class="sxs-lookup"><span data-stu-id="8768f-107">Oozie is integrated with hello Hadoop stack, and it supports hello following jobs:</span></span>

* <span data-ttu-id="8768f-108">Apache MapReduce</span><span class="sxs-lookup"><span data-stu-id="8768f-108">Apache MapReduce</span></span>
* <span data-ttu-id="8768f-109">Apache Pig</span><span class="sxs-lookup"><span data-stu-id="8768f-109">Apache Pig</span></span>
* <span data-ttu-id="8768f-110">Apache Hive</span><span class="sxs-lookup"><span data-stu-id="8768f-110">Apache Hive</span></span>
* <span data-ttu-id="8768f-111">Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="8768f-111">Apache Sqoop</span></span>

<span data-ttu-id="8768f-112">Oozie akkor is, amelyek adott tooa rendszer, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok</span><span class="sxs-lookup"><span data-stu-id="8768f-112">Oozie can also be used tooschedule jobs that are specific tooa system, like Java programs or shell scripts</span></span>

> [!NOTE]
> <span data-ttu-id="8768f-113">A munkafolyamatok és a HDInsight együttes meghatározásához másik lehetőség is Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="8768f-113">Another option for defining workflows with HDInsight is Azure Data Factory.</span></span> <span data-ttu-id="8768f-114">További információ az Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="8768f-114">toolearn more about Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8768f-115">Oozie nincs engedélyezve a HDInsight-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="8768f-115">Oozie is not enabled on domain-joined HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8768f-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8768f-116">Prerequisites</span></span>

* <span data-ttu-id="8768f-117">**HDInsight-fürtök**: lásd: [HDInsight Linux első lépések](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="8768f-117">**An HDInsight cluster**: See [Get Started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="8768f-118">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="8768f-118">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="8768f-119">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="8768f-119">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8768f-120">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8768f-120">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="example-workflow"></a><span data-ttu-id="8768f-121">Példa-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="8768f-121">Example workflow</span></span>

<span data-ttu-id="8768f-122">a dokumentumban használt hello munkafolyamat két műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8768f-122">hello workflow used in this document contains two actions.</span></span> <span data-ttu-id="8768f-123">Műveletek végezhetők a definíciók feladatok, például a Hive, a Sqoop, a MapReduce vagy a másik folyamat fut:</span><span class="sxs-lookup"><span data-stu-id="8768f-123">Actions are definitions for tasks, such as running Hive, Sqoop, MapReduce, or other process:</span></span>

![Munkafolyamat diagramja][img-workflow-diagram]

1. <span data-ttu-id="8768f-125">A Hive művelet a futás HiveQL-parancsfájlt tooextract rögzíti a hello **hivesampletable** HDInsight része.</span><span class="sxs-lookup"><span data-stu-id="8768f-125">A Hive action runs a HiveQL script tooextract records from hello **hivesampletable** included with HDInsight.</span></span> <span data-ttu-id="8768f-126">Minden egyes soraiban levő adatok egy adott mobileszközről meglátogatása ismerteti.</span><span class="sxs-lookup"><span data-stu-id="8768f-126">Each row of data describes a visit from a specific mobile device.</span></span> <span data-ttu-id="8768f-127">hello rekord formátuma a következő szöveg hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-127">hello record format appears similar toohello following text:</span></span>

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    <span data-ttu-id="8768f-128">hello ebben a dokumentumban használt Hive-parancsfájl megjeleníti az egyes platformokon (például Android vagy iPhone) hello teljes látogatások alkalmával, és hello számok tooa új Hive tábla tárolja.</span><span class="sxs-lookup"><span data-stu-id="8768f-128">hello Hive script used in this document counts hello total visits for each platform (such as Android or iPhone) and stores hello counts tooa new Hive table.</span></span>

    <span data-ttu-id="8768f-129">További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="8768f-129">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

2. <span data-ttu-id="8768f-130">A Sqoop művelet hello hello új Hive tábla tooa-táblázat tartalmának Azure SQL adatbázis exportálja.</span><span class="sxs-lookup"><span data-stu-id="8768f-130">A Sqoop action exports hello contents of hello new Hive table tooa table in an Azure SQL database.</span></span> <span data-ttu-id="8768f-131">További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="8768f-131">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="8768f-132">Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello Hadoop-fürt verziók][hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="8768f-132">For supported Oozie versions on HDInsight clusters, see [What's new in hello Hadoop cluster versions provided by HDInsight][hdinsight-versions].</span></span>

## <a name="create-hello-working-directory"></a><span data-ttu-id="8768f-133">Hozzon létre hello munkakönyvtár</span><span class="sxs-lookup"><span data-stu-id="8768f-133">Create hello working directory</span></span>

<span data-ttu-id="8768f-134">Oozie vár a feladat toobe tárolt hello azonos szükséges erőforrások könyvtár.</span><span class="sxs-lookup"><span data-stu-id="8768f-134">Oozie expects resources required for a job toobe stored in hello same directory.</span></span> <span data-ttu-id="8768f-135">Ez a példa **wasb: / / / oktatóanyagok/useoozie**.</span><span class="sxs-lookup"><span data-stu-id="8768f-135">This example uses **wasb:///tutorials/useoozie**.</span></span> <span data-ttu-id="8768f-136">Használja a következő parancs toocreate hello ez, és hello adatok könyvtárat, amely a hello a munkafolyamat által létrehozott új Hive tábla tárolja:</span><span class="sxs-lookup"><span data-stu-id="8768f-136">Use hello following command toocreate this directory, and hello data directory that holds hello new Hive table created by this workflow:</span></span>

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> <span data-ttu-id="8768f-137">Hello `-p` paraméter hello elérési toobe létrehozott összes könyvtár okoz.</span><span class="sxs-lookup"><span data-stu-id="8768f-137">hello `-p` parameter causes all directories in hello path toobe created.</span></span> <span data-ttu-id="8768f-138">Hello **adatok** könyvtár hello által használt használt toohold adatok **useooziewf.hql** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="8768f-138">hello **data** directory is used toohold data used by hello **useooziewf.hql** script.</span></span>

<span data-ttu-id="8768f-139">A következő parancsot, amely biztosítja, hogy a Oozie megszemélyesíthet-e a felhasználói fiók Hive és a Sqoop feladat futtatásakor hello is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="8768f-139">Also run hello following command, which ensures that Oozie can impersonate your user account when running Hive and Sqoop jobs.</span></span> <span data-ttu-id="8768f-140">Cserélje le **felhasználónév** a bejelentkezési névvel:</span><span class="sxs-lookup"><span data-stu-id="8768f-140">Replace **USERNAME** with your login name:</span></span>

```
sudo adduser USERNAME users
```

> [!NOTE]
> <span data-ttu-id="8768f-141">Hibák figyelmen kívül hagyhatja, hogy hello a felhasználó tagja már hello `users` csoport.</span><span class="sxs-lookup"><span data-stu-id="8768f-141">You can ignore errors that hello user is already a member of hello `users` group.</span></span>

## <a name="add-a-database-driver"></a><span data-ttu-id="8768f-142">Egy adatbázis-illesztőprogram hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8768f-142">Add a database driver</span></span>

<span data-ttu-id="8768f-143">Mivel ez a munkafolyamat Sqoop tooexport adatok tooSQL adatbázist használ, meg kell adnia, hello JDBC-illesztőt másolatát tootalk tooSQL adatbázis használja.</span><span class="sxs-lookup"><span data-stu-id="8768f-143">Since this workflow uses Sqoop tooexport data tooSQL Database, you must provide a copy of hello JDBC driver used tootalk tooSQL Database.</span></span> <span data-ttu-id="8768f-144">Használjon hello parancs toocopy következő azt toohello munkakönyvtár:</span><span class="sxs-lookup"><span data-stu-id="8768f-144">Use hello following command toocopy it toohello working directory:</span></span>

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

<span data-ttu-id="8768f-145">Ha a munkafolyamat más erőforrások, például a MapReduce-alkalmazást tartalmazó jar, kellene tooadd ezeket az erőforrásokat is.</span><span class="sxs-lookup"><span data-stu-id="8768f-145">If your workflow used other resources, such as a jar containing a MapReduce application, you would need tooadd these resources as well.</span></span>

## <a name="define-hello-hive-query"></a><span data-ttu-id="8768f-146">Hello Hive-lekérdezés megadása</span><span class="sxs-lookup"><span data-stu-id="8768f-146">Define hello Hive query</span></span>

<span data-ttu-id="8768f-147">A következő HiveQL-parancsfájlt, amely meghatározza az Oozie-munkafolyamat a dokumentum későbbi szakaszában a lekérdezés lépéseket toocreate hello használata.</span><span class="sxs-lookup"><span data-stu-id="8768f-147">Use hello following steps toocreate a HiveQL script that defines a query, which is used in an Oozie workflow later in this document.</span></span>

1. <span data-ttu-id="8768f-148">Csatlakozzon az SSH használatával toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="8768f-148">Connect toohello cluster using SSH.</span></span> <span data-ttu-id="8768f-149">hello parancsban látható egy példa hello segítségével `ssh` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8768f-149">hello following command is an example of using hello `ssh` command.</span></span> <span data-ttu-id="8768f-150">Cserélje le __felhasználónév__ hello fürt hello SSH felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="8768f-150">Replace __USERNAME__ with hello SSH user for hello cluster.</span></span> <span data-ttu-id="8768f-151">Cserélje le __CLUSTERNAME__ hello nevű hello HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="8768f-151">Replace __CLUSTERNAME__ with hello name of hello HDInsight cluster.</span></span>

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="8768f-152">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="8768f-152">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="8768f-153">Hello SSH-kapcsolat alkalmazás a következő parancs toocreate fájl hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-153">From hello SSH connection, use hello following command toocreate a file:</span></span>

    ```
    nano useooziewf.hql
    ```

3. <span data-ttu-id="8768f-154">Amikor megnyílik a hello nano szerkesztő, használja a lekérdezés hello hello fájl tartalmát, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-154">Once hello nano editor opens, use hello following query as hello contents of hello file:</span></span>

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    <span data-ttu-id="8768f-155">Nincsenek hello parancsfájl használt két változót:</span><span class="sxs-lookup"><span data-stu-id="8768f-155">There are two variables used in hello script:</span></span>

    * <span data-ttu-id="8768f-156">**{hiveTableName} $**: hello tábla toobe létrehozott hello nevét tartalmazza</span><span class="sxs-lookup"><span data-stu-id="8768f-156">**${hiveTableName}**: contains hello name of hello table toobe created</span></span>

    * <span data-ttu-id="8768f-157">**{hiveDataFolder} $**: hello hely toostore hello adatfájlok hello tábla tartalmaz</span><span class="sxs-lookup"><span data-stu-id="8768f-157">**${hiveDataFolder}**: contains hello location toostore hello data files for hello table</span></span>

    <span data-ttu-id="8768f-158">hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) továbbítja ezeket az értékeket toothis HiveQL-parancsfájlt futás közben</span><span class="sxs-lookup"><span data-stu-id="8768f-158">hello workflow definition file (workflow.xml in this tutorial) passes these values toothis HiveQL script at run time</span></span>

4. <span data-ttu-id="8768f-159">tooexit hello szerkesztő, nyomja le a Ctrl-X.</span><span class="sxs-lookup"><span data-stu-id="8768f-159">tooexit hello editor, press Ctrl-X.</span></span> <span data-ttu-id="8768f-160">Amikor a rendszer kéri, válassza ki a **Y** toosave hello fájlt, majd használja **Enter** toouse hello **useooziewf.hql** fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="8768f-160">When prompted, select **Y** toosave hello file, then use **Enter** toouse hello **useooziewf.hql** file name.</span></span>

5. <span data-ttu-id="8768f-161">Használjon hello következő parancsok toocopy **useooziewf.hql** túl**wasb:///tutorials/useoozie/useooziewf.hql**:</span><span class="sxs-lookup"><span data-stu-id="8768f-161">Use hello following commands toocopy **useooziewf.hql** too**wasb:///tutorials/useoozie/useooziewf.hql**:</span></span>

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    <span data-ttu-id="8768f-162">Ezek a parancsok tárolására hello **useooziewf.hql** hello fürt hello HDFS-kompatibilis tároló fájlt.</span><span class="sxs-lookup"><span data-stu-id="8768f-162">These commands store hello **useooziewf.hql** file on hello HDFS-compatible storage for hello cluster.</span></span>

## <a name="define-hello-workflow"></a><span data-ttu-id="8768f-163">Adja meg a hello munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="8768f-163">Define hello workflow</span></span>

<span data-ttu-id="8768f-164">Oozie munkafolyamatok definíciók hPDL (az XML folyamat Definition Language) nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="8768f-164">Oozie workflows definitions are written in hPDL (an XML Process Definition Language).</span></span> <span data-ttu-id="8768f-165">A következő lépéseket toodefine hello munkafolyamat hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-165">Use hello following steps toodefine hello workflow:</span></span>

1. <span data-ttu-id="8768f-166">A következő utasítás toocreate hello használja, és szerkeszthet egy új fájlt:</span><span class="sxs-lookup"><span data-stu-id="8768f-166">Use hello following statement toocreate and edit a new file:</span></span>

    ```
    nano workflow.xml
    ```

2. <span data-ttu-id="8768f-167">Ha hello nano szerkesztő nyit, adja meg a hello XML hello fájl tartalmát, a következő:</span><span class="sxs-lookup"><span data-stu-id="8768f-167">Once hello nano editor opens, enter hello following XML as hello file contents:</span></span>

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

    <span data-ttu-id="8768f-168">Nincsenek definiált hello munkafolyamat két műveleteket:</span><span class="sxs-lookup"><span data-stu-id="8768f-168">There are two actions defined in hello workflow:</span></span>

   * <span data-ttu-id="8768f-169">**RunHiveScript**: Ez a művelet hello indítási műveletet, és futtatja a hello **useooziewf.hql** Hive parancsprogram</span><span class="sxs-lookup"><span data-stu-id="8768f-169">**RunHiveScript**: This action is hello start action, and runs hello **useooziewf.hql** Hive script</span></span>

   * <span data-ttu-id="8768f-170">**RunSqoopExport**: Ez a művelet exportálja hello adatok hello Hive parancsfájl tooSQL alapján létrehozott adatbázis-Sqoop használatával.</span><span class="sxs-lookup"><span data-stu-id="8768f-170">**RunSqoopExport**: This action exports hello data created from hello Hive script tooSQL Database using Sqoop.</span></span> <span data-ttu-id="8768f-171">Ez a művelet csak akkor fut, ha hello **RunHiveScript** művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8768f-171">This action only runs if hello **RunHiveScript** action is successful.</span></span>

     <span data-ttu-id="8768f-172">hello munkafolyamat rendelkezik több bejegyzést, például a `${jobTracker}`.</span><span class="sxs-lookup"><span data-stu-id="8768f-172">hello workflow has several entries, such as `${jobTracker}`.</span></span> <span data-ttu-id="8768f-173">Ezek a bejegyzések a hello feladatdefinícióban használt értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="8768f-173">These entries are replaced by values you use in hello job definition.</span></span> <span data-ttu-id="8768f-174">hello feladat meghatározása a jelen dokumentum jön létre.</span><span class="sxs-lookup"><span data-stu-id="8768f-174">hello job definition is created later in this document.</span></span>

     <span data-ttu-id="8768f-175">Megjegyzés: hello is `<archive>sqljdbc4.jar</arcive>` hello Sqoop szakasz bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="8768f-175">Also note hello `<archive>sqljdbc4.jar</arcive>` entry in hello Sqoop section.</span></span> <span data-ttu-id="8768f-176">Ez a bejegyzés utasítja Oozie toomake ebből az archívumból érhető el a Sqoop Ez a művelet futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="8768f-176">This entry instructs Oozie toomake this archive available for Sqoop when this action runs.</span></span>

3. <span data-ttu-id="8768f-177">Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="8768f-177">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

4. <span data-ttu-id="8768f-178">Használjon hello következő parancsot a toocopy hello **workflow.xml** fájl túl**/tutorials/useoozie/workflow.xml**:</span><span class="sxs-lookup"><span data-stu-id="8768f-178">Use hello following command toocopy hello **workflow.xml** file too**/tutorials/useoozie/workflow.xml**:</span></span>

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-hello-database"></a><span data-ttu-id="8768f-179">Hello adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="8768f-179">Create hello database</span></span>

<span data-ttu-id="8768f-180">egy Azure SQL Database toocreate kövesse hello hello [SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8768f-180">toocreate an Azure SQL Database, follow hello steps in hello [Create a SQL Database](../sql-database/sql-database-get-started.md) document.</span></span> <span data-ttu-id="8768f-181">Hello adatbázis létrehozásakor `oozietest` hello adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="8768f-181">When creating hello database, use `oozietest` as hello database name.</span></span> <span data-ttu-id="8768f-182">Továbbá jegyezze fel a hello hello adatbázis-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="8768f-182">Also make a note of hello name of hello database server.</span></span>

### <a name="create-hello-table"></a><span data-ttu-id="8768f-183">Hello tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="8768f-183">Create hello table</span></span>

> [!NOTE]
> <span data-ttu-id="8768f-184">Nincsenek számos módon tooconnect tooSQL adatbázis toocreate egy tábla.</span><span class="sxs-lookup"><span data-stu-id="8768f-184">There are many ways tooconnect tooSQL Database toocreate a table.</span></span> <span data-ttu-id="8768f-185">a következő lépéseket használata hello [FreeTDS](http://www.freetds.org/) hello HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="8768f-185">hello following steps use [FreeTDS](http://www.freetds.org/) from hello HDInsight cluster.</span></span>


1. <span data-ttu-id="8768f-186">Parancs tooinstall FreeTDS következő hello HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-186">Use hello following command tooinstall FreeTDS on hello HDInsight cluster:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. <span data-ttu-id="8768f-187">Amennyiben telepítve van a FreeTDS, használja a következő parancs tooconnect toohello SQL-adatbáziskiszolgáló korábban létrehozott hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-187">Once FreeTDS has been installed, use hello following command tooconnect toohello SQL Database server you created previously:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="8768f-188">Kimeneti hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-188">You receive output similar toohello following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set toooozietest
        1>

3. <span data-ttu-id="8768f-189">A hello `1>` kéri, adja meg az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-189">At hello `1>` prompt, enter hello following lines:</span></span>

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    <span data-ttu-id="8768f-190">Ha hello `GO` utasításban is meg kell adni, hello előző utasítás kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="8768f-190">When hello `GO` statement is entered, hello previous statements are evaluated.</span></span> <span data-ttu-id="8768f-191">Ezekre az utasításokra, hozzon létre egy táblát nevű **mobiledata** hello munkafolyamat által használt.</span><span class="sxs-lookup"><span data-stu-id="8768f-191">These statements create a table named **mobiledata** that is used by hello workflow.</span></span>

    <span data-ttu-id="8768f-192">A következő tooverify, amelyek a tábla hello használata hello jött létre:</span><span class="sxs-lookup"><span data-stu-id="8768f-192">Use hello following tooverify that hello table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="8768f-193">Kimeneti hasonló toohello a következő szöveg jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-193">You see output similar toohello following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. <span data-ttu-id="8768f-194">Adja meg `exit` : hello `1>` tooexit hello tsql segédprogram kérni.</span><span class="sxs-lookup"><span data-stu-id="8768f-194">Enter `exit` at hello `1>` prompt tooexit hello tsql utility.</span></span>

## <a name="create-hello-job-definition"></a><span data-ttu-id="8768f-195">Hello feladatdefiníció létrehozása</span><span class="sxs-lookup"><span data-stu-id="8768f-195">Create hello job definition</span></span>

<span data-ttu-id="8768f-196">hello feladatdefiníció írja le, ahol toofind hello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="8768f-196">hello job definition describes where toofind hello workflow.xml.</span></span> <span data-ttu-id="8768f-197">Azt is bemutatja where toofind más hello munkafolyamat (például useooziewf.hql.) által használt fájlokat Azt is meghatározza hello értékek tulajdonságok hello munkafolyamaton belül használt, és a kapcsolódó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="8768f-197">It also describes where toofind other files used by hello workflow (such as useooziewf.hql.) It also defines hello values for properties used within hello workflow and associated files.</span></span>

1. <span data-ttu-id="8768f-198">A következő parancs tooget hello teljes címe hello alapértelmezett tárolási hello használata.</span><span class="sxs-lookup"><span data-stu-id="8768f-198">Use hello following command tooget hello full address of hello default storage.</span></span> <span data-ttu-id="8768f-199">Ez a cím néhány percet szerepel hello konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="8768f-199">This address is used in hello configuration file in a moment:</span></span>

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    <span data-ttu-id="8768f-200">Ez a parancs visszaadja az adatokat hasonló toohello XML a következő:</span><span class="sxs-lookup"><span data-stu-id="8768f-200">This command returns information similar toohello following XML:</span></span>

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > <span data-ttu-id="8768f-201">Ha a HDInsight-fürt hello Azure Storage hello alapértelmezett tárolóként használ, hello `<value>` elem tartalma kezdődnie `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="8768f-201">If hello HDInsight cluster uses Azure Storage as hello default storage, hello `<value>` element contents begin with `wasb://`.</span></span> <span data-ttu-id="8768f-202">Ha az Azure Data Lake Store használata esetén kezdődik `adl://`.</span><span class="sxs-lookup"><span data-stu-id="8768f-202">If Azure Data Lake Store is used instead, it begins with `adl://`.</span></span>

    <span data-ttu-id="8768f-203">Mentés hello hello tartalmát `<value>` elem, mert a hello lépések használatban van.</span><span class="sxs-lookup"><span data-stu-id="8768f-203">Save hello contents of hello `<value>` element, as it is used in hello next steps.</span></span>

2. <span data-ttu-id="8768f-204">A következő parancs tooget hello fürt headnode teljes Tartományneve hello használata.</span><span class="sxs-lookup"><span data-stu-id="8768f-204">Use hello following command tooget FQDN of hello cluster headnode.</span></span> <span data-ttu-id="8768f-205">Ez az információ hello JobTracker címet hello fürtnek szolgál:</span><span class="sxs-lookup"><span data-stu-id="8768f-205">This information is used for hello JobTracker address for hello cluster:</span></span>

    ```
    hostname -f
    ```

    <span data-ttu-id="8768f-206">Ez visszaadja a szöveg a következő információk hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="8768f-206">This returns information similar toohello following text:</span></span>

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    <span data-ttu-id="8768f-207">hello hello JobTracker használt port 8050, így a Hadoopból hello hello teljes címe toouse van `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span><span class="sxs-lookup"><span data-stu-id="8768f-207">hello port used for hello JobTracker is 8050, so hello full address toouse for hello JobTracker is `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span></span>

3. <span data-ttu-id="8768f-208">A következő toocreate hello Oozie feladat definition konfigurációs hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-208">Use hello following toocreate hello Oozie job definition configuration:</span></span>

    ```
    nano job.xml
    ```

4. <span data-ttu-id="8768f-209">Amikor megnyílik a hello nano szerkesztő, használja a hello XML hello hello fájl tartalmát, a következő:</span><span class="sxs-lookup"><span data-stu-id="8768f-209">Once hello nano editor opens, use hello following XML as hello contents of hello file:</span></span>

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

   * <span data-ttu-id="8768f-210">Cserélje le az összes példányát  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  alapértelmezett tárolási korábban fogadott hello értékkel.</span><span class="sxs-lookup"><span data-stu-id="8768f-210">Replace all instances of **wasb://mycontainer@mystorageaccount.blob.core.windows.net** with hello value you received earlier for default storage.</span></span>

     > [!WARNING]
     > <span data-ttu-id="8768f-211">Ha hello elérési út egy `wasb` elérési útja, hello teljes elérési útja kell használnia.</span><span class="sxs-lookup"><span data-stu-id="8768f-211">If hello path is a `wasb` path, you must use hello full path.</span></span> <span data-ttu-id="8768f-212">Nem rövidíthető az toojust `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="8768f-212">Do not shorten it toojust `wasb:///`.</span></span>

   * <span data-ttu-id="8768f-213">Cserélje le **JOBTRACKERADDRESS** a korábban fogadott JobTracker/ResourceManager cím hello.</span><span class="sxs-lookup"><span data-stu-id="8768f-213">Replace **JOBTRACKERADDRESS** with hello JobTracker/ResourceManager address you received earlier.</span></span>
   * <span data-ttu-id="8768f-214">Cserélje le **sajatNev** hello HDInsight-fürthöz, a bejelentkezési névvel.</span><span class="sxs-lookup"><span data-stu-id="8768f-214">Replace **YourName** with your login name for hello HDInsight cluster.</span></span>
   * <span data-ttu-id="8768f-215">Cserélje le **kiszolgálónév**, **adminLogin**, és **adminPassword** hello információkkal, illetve az Azure SQL Database adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="8768f-215">Replace **serverName**, **adminLogin**, and **adminPassword** with hello information for your Azure SQL Database.</span></span>

     <span data-ttu-id="8768f-216">A legtöbb hello információkat ebben a fájlban használt toopopulate hello értékek fájlokban használt hello workflow.xml vagy ooziewf.hql (például ${nameNode}.)</span><span class="sxs-lookup"><span data-stu-id="8768f-216">Most of hello information in this file is used toopopulate hello values used in hello workflow.xml or ooziewf.hql files (such as ${nameNode}.)</span></span>

     > [!NOTE]
     > <span data-ttu-id="8768f-217">Hello **oozie.wf.application.path** a bejegyzés határozza meg, ha a feladat futott toofind hello workflow.xml fájl, amely tartalmazza a hello munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="8768f-217">hello **oozie.wf.application.path** entry defines where toofind hello workflow.xml file, which contains hello workflow ran by this job.</span></span>

5. <span data-ttu-id="8768f-218">Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="8768f-218">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

## <a name="submit-and-manage-hello-job"></a><span data-ttu-id="8768f-219">Küldje el, majd hello feladat kezelése</span><span class="sxs-lookup"><span data-stu-id="8768f-219">Submit and manage hello job</span></span>

<span data-ttu-id="8768f-220">hello lépések használata hello Oozie parancs toosubmit és kezelése Oozie munkafolyamatok hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="8768f-220">hello following steps use hello Oozie command toosubmit and manage Oozie workflows on hello cluster.</span></span> <span data-ttu-id="8768f-221">hello Oozie parancs egy egyszerű felületen keresztül hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="8768f-221">hello Oozie command is a friendly interface over hello [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8768f-222">Hello Oozie parancs használata esetén a hello FQDN a hello HDInsight headnode kell használnia.</span><span class="sxs-lookup"><span data-stu-id="8768f-222">When using hello Oozie command, you must use hello FQDN for hello HDInsight headnode.</span></span> <span data-ttu-id="8768f-223">Ez a teljes tartománynév csak elérhető hello fürtből, vagy ha hello fürt hello a más számítógépről egy Azure virtuális hálózat ugyanazon a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="8768f-223">This FQDN is only accessible from hello cluster, or if hello cluster is on an Azure Virtual Network, from other machines on hello same network.</span></span>


1. <span data-ttu-id="8768f-224">A következő tooobtain hello URL-cím toohello Oozie szolgáltatás hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-224">Use hello following tooobtain hello URL toohello Oozie service:</span></span>

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    <span data-ttu-id="8768f-225">Ez visszaad információk hasonló toohello XML a következő:</span><span class="sxs-lookup"><span data-stu-id="8768f-225">This returns information similar toohello following XML:</span></span>

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    <span data-ttu-id="8768f-226">Hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` részét hello URL-cím toouse hello Oozie parancsot a rendszer.</span><span class="sxs-lookup"><span data-stu-id="8768f-226">hello `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` portion is hello URL toouse with hello Oozie command.</span></span>

2. <span data-ttu-id="8768f-227">Következő hello URL toocreate egy környezeti változó, így nem kell tootype használata hello azt minden parancs:</span><span class="sxs-lookup"><span data-stu-id="8768f-227">Use hello following toocreate an environment variable for hello URL, so you don't have tootype it for every command:</span></span>

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    <span data-ttu-id="8768f-228">Cserélje le egy korábban fogadott hello hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8768f-228">Replace hello URL with hello one you received earlier.</span></span>
3. <span data-ttu-id="8768f-229">A következő toosubmit hello feladat hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-229">Use hello following toosubmit hello job:</span></span>

    ```
    oozie job -config job.xml -submit
    ```

    <span data-ttu-id="8768f-230">A parancs betölt hello feladat adatait **job.xml** és elküldi tooOozie, de nem futtatható biztosítja.</span><span class="sxs-lookup"><span data-stu-id="8768f-230">This command loads hello job information from **job.xml** and submits it tooOozie, but does not run it.</span></span>

    <span data-ttu-id="8768f-231">Hello parancs után az hello feladat hello Azonosítóját kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="8768f-231">Once hello command completes, it should return hello ID of hello job.</span></span> <span data-ttu-id="8768f-232">Például: `0000005-150622124850154-oozie-oozi-W`.</span><span class="sxs-lookup"><span data-stu-id="8768f-232">For example, `0000005-150622124850154-oozie-oozi-W`.</span></span> <span data-ttu-id="8768f-233">Ez az azonosító is használt toomanage hello feladat.</span><span class="sxs-lookup"><span data-stu-id="8768f-233">This ID is used toomanage hello job.</span></span>

4. <span data-ttu-id="8768f-234">A következő parancs hello hello feladat hello állapotának megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="8768f-234">View hello status of hello job using hello following command:</span></span>

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > <span data-ttu-id="8768f-235">Cserélje le `<JOBID>` hello az azonosító hello előző lépés eredményeképpen visszakapott.</span><span class="sxs-lookup"><span data-stu-id="8768f-235">Replace `<JOBID>` with hello ID returned in hello previous step.</span></span>

    <span data-ttu-id="8768f-236">Ez visszaadja a szöveg a következő információk hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="8768f-236">This returns information similar toohello following text:</span></span>

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

    <span data-ttu-id="8768f-237">Ez a feladat állapota `PREP`.</span><span class="sxs-lookup"><span data-stu-id="8768f-237">This job has a status of `PREP`.</span></span> <span data-ttu-id="8768f-238">Ez azt jelzi, hogy hello feladat lett létrehozva, de nem indult el.</span><span class="sxs-lookup"><span data-stu-id="8768f-238">This status indicates that hello job was created, but not started.</span></span>

5. <span data-ttu-id="8768f-239">A következő parancs toostart hello feladat hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-239">Use hello following command toostart hello job:</span></span>

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > <span data-ttu-id="8768f-240">Cserélje le `<JOBID>` hello az azonosító adott vissza korábban.</span><span class="sxs-lookup"><span data-stu-id="8768f-240">Replace `<JOBID>` with hello ID returned previously.</span></span>

    <span data-ttu-id="8768f-241">Ha hello állapotát a parancs után ellenőrizze, hogy futó állapotban van, és hello feladat hello műveleteire vonatkozó információk jelennek.</span><span class="sxs-lookup"><span data-stu-id="8768f-241">If you check hello status after this command, it is in a running state, and information is returned for hello actions within hello job.</span></span>

6. <span data-ttu-id="8768f-242">Hello feladat sikeres befejeződése után ellenőrizheti, hogy hello adatok jött létre, és toohello SQL-adatbázis táblája hello a következő parancsok futtatásával exportált:</span><span class="sxs-lookup"><span data-stu-id="8768f-242">Once hello task completes successfully, you can verify that hello data was generated and exported toohello SQL Database table by using hello following commands:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="8768f-243">A hello `1>` kéri, adja meg a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-243">At hello `1>` prompt, enter hello following query:</span></span>

    ```
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="8768f-244">hello adatokat adott vissza a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="8768f-244">hello information returned is similar toohello following text:</span></span>

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

<span data-ttu-id="8768f-245">Hello Oozie parancs további információkért lásd: [Oozie parancssori eszköz](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span><span class="sxs-lookup"><span data-stu-id="8768f-245">For more information on hello Oozie command, see [Oozie command-line tool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span></span>

## <a name="oozie-rest-api"></a><span data-ttu-id="8768f-246">Oozie REST API-n</span><span class="sxs-lookup"><span data-stu-id="8768f-246">Oozie REST API</span></span>

<span data-ttu-id="8768f-247">hello Oozie REST API toobuild lehetővé teszi a saját eszközök Oozie együtt használható.</span><span class="sxs-lookup"><span data-stu-id="8768f-247">hello Oozie REST API allows you toobuild your own tools that work with Oozie.</span></span> <span data-ttu-id="8768f-248">Az alábbiakban hello HDInsight hello Oozie REST API használatával kapcsolatos információkat:</span><span class="sxs-lookup"><span data-stu-id="8768f-248">hello following are HDInsight specific information about using hello Oozie REST API:</span></span>

* <span data-ttu-id="8768f-249">**URI**: hello REST API-n elérhető külső hello fürthöz`https://CLUSTERNAME.azurehdinsight.net/oozie`</span><span class="sxs-lookup"><span data-stu-id="8768f-249">**URI**: hello REST API can be accessed from outside hello cluster at `https://CLUSTERNAME.azurehdinsight.net/oozie`</span></span>

* <span data-ttu-id="8768f-250">**Hitelesítési**: toohello API hello fürt HTTP-fiókkal (rendszergazda) és a jelszó hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8768f-250">**Authentication**: Authenticate toohello API using hello cluster HTTP account (admin) and password.</span></span> <span data-ttu-id="8768f-251">Példa:</span><span class="sxs-lookup"><span data-stu-id="8768f-251">For example:</span></span>

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

<span data-ttu-id="8768f-252">Hello Oozie REST API használatával kapcsolatos további információkért lásd: [Oozie webszolgáltatási API-ra](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="8768f-252">For more information on using hello Oozie REST API, see [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

## <a name="oozie-web-ui"></a><span data-ttu-id="8768f-253">Oozie webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="8768f-253">Oozie Web UI</span></span>

<span data-ttu-id="8768f-254">hello Oozie webes felhasználói felület révén Oozie feladatok állapotának hello webes képet hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="8768f-254">hello Oozie Web UI provides a web-based view into hello status of Oozie jobs on hello cluster.</span></span> <span data-ttu-id="8768f-255">hello webes felhasználói felület lehetővé teszi a következő információ tooview hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-255">hello web UI allows you tooview hello following information:</span></span>

* <span data-ttu-id="8768f-256">Feladat állapota</span><span class="sxs-lookup"><span data-stu-id="8768f-256">Job status</span></span>
* <span data-ttu-id="8768f-257">Feladatdefiníció</span><span class="sxs-lookup"><span data-stu-id="8768f-257">Job definition</span></span>
* <span data-ttu-id="8768f-258">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="8768f-258">Configuration</span></span>
* <span data-ttu-id="8768f-259">Hello műveletek hello feladat grafikonja</span><span class="sxs-lookup"><span data-stu-id="8768f-259">A graph of hello actions in hello job</span></span>
* <span data-ttu-id="8768f-260">Naplók hello feladat</span><span class="sxs-lookup"><span data-stu-id="8768f-260">Logs for hello job</span></span>

<span data-ttu-id="8768f-261">Műveletek a feladat részleteit is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="8768f-261">You can also view details for actions within a job.</span></span>

<span data-ttu-id="8768f-262">tooaccess hello Oozie webes felhasználói felületén, a következő lépéseket hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-262">tooaccess hello Oozie Web UI, use hello following steps:</span></span>

1. <span data-ttu-id="8768f-263">Hozzon létre egy SSH-alagút toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8768f-263">Create an SSH tunnel toohello HDInsight cluster.</span></span> <span data-ttu-id="8768f-264">További információ: hello [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="8768f-264">For information, see hello [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="8768f-265">Ha alagút létrejött, a böngészőben nyissa meg a hello Ambari webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="8768f-265">Once a tunnel has been created, open hello Ambari web UI in your web browser.</span></span> <span data-ttu-id="8768f-266">hello URI hello Ambari hely van **https://CLUSTERNAME.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="8768f-266">hello URI for hello Ambari site is **https://CLUSTERNAME.azurehdinsight.net**.</span></span> <span data-ttu-id="8768f-267">Cserélje le **CLUSTERNAME** hello nevet, a Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8768f-267">Replace **CLUSTERNAME** with hello name of your Linux-based HDInsight cluster.</span></span>

3. <span data-ttu-id="8768f-268">Válassza ki a hello bal oldalán található hello, **Oozie**, majd **Gyorshivatkozások**, és végül **Oozie webes felhasználói felületén**.</span><span class="sxs-lookup"><span data-stu-id="8768f-268">From hello left side of hello page, select **Oozie**, then **Quick Links**, and finally **Oozie Web UI**.</span></span>

    ![hello menük képe](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. <span data-ttu-id="8768f-270">hello Oozie webes felhasználói felületének alapértelmezett toodisplaying rendszert futtató munkafolyamat-feladatokat.</span><span class="sxs-lookup"><span data-stu-id="8768f-270">hello Oozie Web UI defaults toodisplaying running Workflow Jobs.</span></span> <span data-ttu-id="8768f-271">toosee összes munkafolyamat-feladatokat, válassza ki **összes feladat**.</span><span class="sxs-lookup"><span data-stu-id="8768f-271">toosee all workflow jobs, select **All Jobs**.</span></span>

    ![Minden feladat jelenik meg](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. <span data-ttu-id="8768f-273">Válassza ki a feladat tooview hello feladat további információt.</span><span class="sxs-lookup"><span data-stu-id="8768f-273">Select a job tooview more information about hello job.</span></span>

    ![Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. <span data-ttu-id="8768f-275">Hello Feladatinformáció lapján tekintheti meg egyszerű feladat információi és hello egyes műveletek hello feladat.</span><span class="sxs-lookup"><span data-stu-id="8768f-275">From hello Job Info tab, you can see basic job information and hello individual actions within hello job.</span></span> <span data-ttu-id="8768f-276">Hello lapok használatával megtekintheti hello felső feladatdefiníció, konfigurációs feladatok, hozzáférési hello Job Log hello, vagy egy irányított aciklikus diagramhoz (DAG) hello feladat megtekintése.</span><span class="sxs-lookup"><span data-stu-id="8768f-276">Using hello tabs at hello top you can view hello Job Definition, Job Configuration, access hello Job Log or view a Directed Acyclic Graph (DAG) of hello job.</span></span>

   * <span data-ttu-id="8768f-277">**Job Log**: Select hello **GetLogs** tooget hello feladathoz tartozó összes naplófájlt, illetve programmal hello **keresési szűrőt adjon meg** toofilter naplók mezőben</span><span class="sxs-lookup"><span data-stu-id="8768f-277">**Job Log**: Select hello **GetLogs** button tooget all logs for hello job, or use hello **Enter Search Filter** field toofilter logs</span></span>

       ![Feladat-napló](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * <span data-ttu-id="8768f-279">**JobDAG**: hello DAG hello adatelérési utak hello munkafolyamaton keresztül végrehajtott grafikus áttekintése</span><span class="sxs-lookup"><span data-stu-id="8768f-279">**JobDAG**: hello DAG is a graphical overview of hello data paths taken through hello workflow</span></span>

       ![Feladat DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. <span data-ttu-id="8768f-281">Egy hello művelet kijelölésével hello **Feladatinformáció** lapon megjelenik információk hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="8768f-281">Selecting one of hello actions from hello **Job Info** tab brings up information for hello action.</span></span> <span data-ttu-id="8768f-282">Válassza például a hello **RunHiveScript** művelet.</span><span class="sxs-lookup"><span data-stu-id="8768f-282">For example, select hello **RunHiveScript** action.</span></span>

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/action.png)

8. <span data-ttu-id="8768f-284">Láthatja a részletes hello műveletben, például egy hivatkozás toohello **konzol URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="8768f-284">You can see details for hello action, such as a link toohello **Console URL**.</span></span> <span data-ttu-id="8768f-285">Ez a hivatkozás használt tooview JobTracker információk hello feladat lehet.</span><span class="sxs-lookup"><span data-stu-id="8768f-285">This link can be used tooview JobTracker information for hello job.</span></span>

## <a name="scheduling-jobs"></a><span data-ttu-id="8768f-286">Feladatütemezés</span><span class="sxs-lookup"><span data-stu-id="8768f-286">Scheduling jobs</span></span>

<span data-ttu-id="8768f-287">hello koordinátor lehetővé teszi az start, záró és előfordulási gyakoriságát feladatok toospecify.</span><span class="sxs-lookup"><span data-stu-id="8768f-287">hello coordinator allows you toospecify a start, end, and occurrence frequency for jobs.</span></span> <span data-ttu-id="8768f-288">toodefine hello munkafolyamat, a lépéseket követve használata hello ütemezés szerint:</span><span class="sxs-lookup"><span data-stu-id="8768f-288">toodefine a schedule for hello workflow, use hello following steps:</span></span>

1. <span data-ttu-id="8768f-289">A következő toocreate nevű fájl használatát hello **coordinator.xml**:</span><span class="sxs-lookup"><span data-stu-id="8768f-289">Use hello following toocreate a file named **coordinator.xml**:</span></span>

    ```
    nano coordinator.xml
    ```

    <span data-ttu-id="8768f-290">Hello XML hello hello fájl tartalmát, a következő használja:</span><span class="sxs-lookup"><span data-stu-id="8768f-290">Use hello following XML as hello contents of hello file:</span></span>

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
    > <span data-ttu-id="8768f-291">Hello `${...}` változók futásidőben hello feladatdefiníció értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="8768f-291">hello `${...}` variables are replaced by values in hello job definition at run-time.</span></span> <span data-ttu-id="8768f-292">hello változók az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="8768f-292">hello variables are:</span></span>
    >
    > * <span data-ttu-id="8768f-293">`${coordFrequency}`: Az hello feladat futó példányait között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="8768f-293">`${coordFrequency}`: Time between running instances of hello job.</span></span>
    > <span data-ttu-id="8768f-294">** `${coordStart}`: hello feladat kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="8768f-294">** `${coordStart}`: hello job start time.</span></span>
    > * <span data-ttu-id="8768f-295">`${coordEnd}`: hello feladat befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="8768f-295">`${coordEnd}`: hello job end time.</span></span>
    > * <span data-ttu-id="8768f-296">`${coordTimezone}`: Koordinátor feladatok vannak rögzített időzónában nem nyári időszámításra (általában képviseli UTC használatával).</span><span class="sxs-lookup"><span data-stu-id="8768f-296">`${coordTimezone}`: Coordinator jobs are in a fixed time zone with no daylight savings time (typically represented by using UTC).</span></span> <span data-ttu-id="8768f-297">Ez az időzóna kezeli hello "Oozie feldolgozási időzóna."</span><span class="sxs-lookup"><span data-stu-id="8768f-297">This time zone is referred as hello "Oozie processing timezone."</span></span>
    > * <span data-ttu-id="8768f-298">`${wfPath}`: hello elérési toohello workflow.xml.</span><span class="sxs-lookup"><span data-stu-id="8768f-298">`${wfPath}`: hello path toohello workflow.xml.</span></span>

2. <span data-ttu-id="8768f-299">toosave hello fájl létrehozásához használja a Ctrl-X, **Y**, és **Enter**.</span><span class="sxs-lookup"><span data-stu-id="8768f-299">toosave hello file, use Ctrl-X, **Y**, and **Enter**.</span></span>

3. <span data-ttu-id="8768f-300">A következő parancs toocopy hello fájl toohello munkakönyvtár megadása a feladat hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-300">Use hello following command toocopy hello file toohello working directory for this job:</span></span>

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. <span data-ttu-id="8768f-301">A következő toomodify hello használata hello **job.xml** fájlt:</span><span class="sxs-lookup"><span data-stu-id="8768f-301">Use hello following toomodify hello **job.xml** file:</span></span>

    ```
    nano job.xml
    ```

    <span data-ttu-id="8768f-302">Hajtsa végre a következő módosításokat hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-302">Make hello following changes:</span></span>

   * <span data-ttu-id="8768f-303">tooinstruct oozie toorun hello koordinátor fájl helyett hello munkafolyamat, módosítás `<name>oozie.wf.application.path</name>` túl`<name>oozie.coord.application.path</name>`.</span><span class="sxs-lookup"><span data-stu-id="8768f-303">tooinstruct oozie toorun hello coordinator file instead of hello workflow, change `<name>oozie.wf.application.path</name>` too`<name>oozie.coord.application.path</name>`.</span></span>

   * <span data-ttu-id="8768f-304">tooset hello `workflowPath` hello koordinátor által használt változó hozzáadása a következő XML hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-304">tooset hello `workflowPath` variable used by hello coordinator, add hello following XML:</span></span>

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       <span data-ttu-id="8768f-305">Cserélje le a hello `wasb://mycontainer@mystorageaccount.blob.core.windows` hello értékkel más bejegyzések hello job.xml fájlban használt szöveg.</span><span class="sxs-lookup"><span data-stu-id="8768f-305">Replace hello `wasb://mycontainer@mystorageaccount.blob.core.windows` text with hello value used in other entries in hello job.xml file.</span></span>

   * <span data-ttu-id="8768f-306">toodefine hello start, záró és hello koordinátor gyakorisága adja hozzá a következő XML hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-306">toodefine hello start, end, and frequency for hello coordinator, add hello following XML:</span></span>

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

       <span data-ttu-id="8768f-307">Ezek az értékek beállítása hello kezdési idő too12: 00 PM 2017. Előfordulhat, hogy 10., a befejezési idő tooMay 12, hello 2017.</span><span class="sxs-lookup"><span data-stu-id="8768f-307">These values set hello start time too12:00PM on May 10, 2017, hello end time tooMay 12, 2017.</span></span> <span data-ttu-id="8768f-308">Ez a feladat futtatása naponta hello intervallumát.</span><span class="sxs-lookup"><span data-stu-id="8768f-308">hello interval for running this job daily.</span></span> <span data-ttu-id="8768f-309">hello gyakorisága (percben), akkor úgy 24 óra x 60 perc = 1440 perc.</span><span class="sxs-lookup"><span data-stu-id="8768f-309">hello frequency is in minutes, so 24 hours x 60 minutes = 1440 minutes.</span></span> <span data-ttu-id="8768f-310">Végezetül hello időzóna tooUTC van beállítva.</span><span class="sxs-lookup"><span data-stu-id="8768f-310">Finally, hello timezone is set tooUTC.</span></span>

5. <span data-ttu-id="8768f-311">Használja a Ctrl-X, majd **Y** és **Enter** toosave hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="8768f-311">Use Ctrl-X, then **Y** and **Enter** toosave hello file.</span></span>

6. <span data-ttu-id="8768f-312">toorun hello feladat, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-312">toorun hello job, use hello following command:</span></span>

    ```
    oozie job -config job.xml -run
    ```

    <span data-ttu-id="8768f-313">Ez a parancs küldi el, és elindítja hello feladatot.</span><span class="sxs-lookup"><span data-stu-id="8768f-313">This command submits and starts hello job.</span></span>

7. <span data-ttu-id="8768f-314">Ha látogasson el a hello Oozie webes felhasználói felületén, és válassza ki a hello **koordinátor feladatok** lapon látható információk hasonló toohello kép a következő:</span><span class="sxs-lookup"><span data-stu-id="8768f-314">If you visit hello Oozie Web UI and select hello **Coordinator Jobs** tab, you see information similar toohello following image:</span></span>

    ![koordinátor feladatok lap](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    <span data-ttu-id="8768f-316">Hello **következő Materialization** -bejegyzése tartalmazza hello, amikor legközelebb hello feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="8768f-316">hello **Next Materialization** entry contains hello next time that hello job runs.</span></span>

8. <span data-ttu-id="8768f-317">Hasonló toohello korábbi munkafolyamat-feladat, kiválasztja hello feladat bejegyzés hello webes felhasználói felület hello feladattal kapcsolatos információkat jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-317">Similar toohello earlier workflow job, selecting hello job entry in hello web UI displays information on hello job:</span></span>

    ![Koordinátor Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > <span data-ttu-id="8768f-319">Ez a rendszerkép csak hello feladat sikeres futtatása ütemezett hello munkafolyamaton belül nem az egyéni műveletek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8768f-319">This image only shows successful runs of hello job, not individual actions within hello scheduled workflow.</span></span> <span data-ttu-id="8768f-320">Ezt követően válasszon ki egy hello toosee **művelet** bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="8768f-320">toosee that, select one of hello **Action** entries.</span></span>

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a><span data-ttu-id="8768f-322">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="8768f-322">Troubleshooting</span></span>

<span data-ttu-id="8768f-323">hello Oozie felhasználói felület lehetővé teszi a tooview Oozie naplókat.</span><span class="sxs-lookup"><span data-stu-id="8768f-323">hello Oozie UI allows you tooview Oozie logs.</span></span> <span data-ttu-id="8768f-324">Hivatkozások tooJobTracker naplók hello munkafolyamat indította MapReduce-feladatok is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="8768f-324">It also contains links tooJobTracker logs for MapReduce tasks started by hello workflow.</span></span> <span data-ttu-id="8768f-325">hibaelhárítási hello mintát kell lennie:</span><span class="sxs-lookup"><span data-stu-id="8768f-325">hello pattern for troubleshooting should be:</span></span>

1. <span data-ttu-id="8768f-326">Hello feladat megtekintése Oozie webes felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="8768f-326">View hello job in Oozie Web UI.</span></span>

2. <span data-ttu-id="8768f-327">Ha egy hiba vagy egy adott művelet sikertelen, válassza ki a művelet toosee hello, ha hello **hibaüzenet** mező hello hiba nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="8768f-327">If there is an error or failure for a specific action, select hello action toosee if hello **Error Message** field provides more information on hello failure.</span></span>

3. <span data-ttu-id="8768f-328">Ha elérhető, használja hello művelet tooview hello URL-CÍMÉT (például a Hadoopból naplók) további részleteket hello a művelethez.</span><span class="sxs-lookup"><span data-stu-id="8768f-328">If available, use hello URL from hello action tooview more details (such as JobTracker logs) for hello action.</span></span>

<span data-ttu-id="8768f-329">hello az alábbiakban felmerülhet, bizonyos hibákat, és hogyan tooresolve őket.</span><span class="sxs-lookup"><span data-stu-id="8768f-329">hello following are specific errors you may encounter, and how tooresolve them.</span></span>

### <a name="ja009-cannot-initialize-cluster"></a><span data-ttu-id="8768f-330">JA009: Nem lehet inicializálni a fürt</span><span class="sxs-lookup"><span data-stu-id="8768f-330">JA009: Cannot initialize cluster</span></span>

<span data-ttu-id="8768f-331">**A jelenség**: hello túl a feladat állapotának megváltozásakor**FELFÜGGESZTETT**.</span><span class="sxs-lookup"><span data-stu-id="8768f-331">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="8768f-332">Hello feladat részleteinek megjelenítése hello RunHiveScript állapotának **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="8768f-332">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="8768f-333">Látható értesítések valamelyikének kiválasztásakor hello művelet a következő hibaüzenet hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-333">Selecting hello action displays hello following error message:</span></span>

    JA009: Cannot initialize Cluster. Please check your configuration for map

<span data-ttu-id="8768f-334">**OK**: hello WASB címekre hello **job.xml** fájl tartalmaz hello tároló vagy a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="8768f-334">**Cause**: hello WASB addresses used in hello **job.xml** file do not contain hello storage container or storage account name.</span></span> <span data-ttu-id="8768f-335">hello WASB cím formátumban kell megadni `wasb://containername@storageaccountname.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="8768f-335">hello WASB address format must be `wasb://containername@storageaccountname.blob.core.windows.net`.</span></span>

<span data-ttu-id="8768f-336">**Megoldási**: hello feladat által használt hello WASB címek módosítása.</span><span class="sxs-lookup"><span data-stu-id="8768f-336">**Resolution**: Change hello WASB addresses used by hello job.</span></span>

### <a name="ja002-oozie-is-not-allowed-tooimpersonate-ltuser"></a><span data-ttu-id="8768f-337">JA002: A Oozie nem engedélyezett a tooimpersonate &lt;felhasználó ></span><span class="sxs-lookup"><span data-stu-id="8768f-337">JA002: Oozie is not allowed tooimpersonate &lt;USER></span></span>

<span data-ttu-id="8768f-338">**A jelenség**: hello túl a feladat állapotának megváltozásakor**FELFÜGGESZTETT**.</span><span class="sxs-lookup"><span data-stu-id="8768f-338">**Symptoms**: hello job status changes too**SUSPENDED**.</span></span> <span data-ttu-id="8768f-339">Hello feladat részleteinek megjelenítése hello RunHiveScript állapotának **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="8768f-339">Details for hello job show hello RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="8768f-340">A következő hibaüzenet hello hello művelet kiválasztása jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-340">Selecting hello action shows hello following error message:</span></span>

    JA002: User: oozie is not allowed tooimpersonate <USER>

<span data-ttu-id="8768f-341">**OK**: aktuális engedély-beállítások nem teszik lehetővé az Oozie tooimpersonate hello adott felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="8768f-341">**Cause**: Current permission settings do not allow Oozie tooimpersonate hello specified user account.</span></span>

<span data-ttu-id="8768f-342">**Megoldási**: Oozie tooimpersonate felhasználók engedélyezett hello **felhasználók** csoport.</span><span class="sxs-lookup"><span data-stu-id="8768f-342">**Resolution**: Oozie is allowed tooimpersonate users in hello **users** group.</span></span> <span data-ttu-id="8768f-343">Használjon hello `groups USERNAME` , amely a felhasználói fiók hello toosee hello csoportok tagja.</span><span class="sxs-lookup"><span data-stu-id="8768f-343">Use hello `groups USERNAME` toosee hello groups that hello user account is a member of.</span></span> <span data-ttu-id="8768f-344">Ha hello felhasználó nem tagja hello **felhasználók** csoportjában a következő parancs toohello tooadd hello felhasználócsoport hello használata:</span><span class="sxs-lookup"><span data-stu-id="8768f-344">If hello user is not a member of hello **users** group, use hello following command tooadd hello user toohello group:</span></span>

    sudo adduser USERNAME users

> [!NOTE]
> <span data-ttu-id="8768f-345">HDInsight felismeri, hogy a felhasználó hello hozzáadott toohello csoport több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="8768f-345">It may take several minutes before HDInsight recognizes that hello user has been added toohello group.</span></span>

### <a name="launcher-error-sqoop"></a><span data-ttu-id="8768f-346">Hiba (Sqoop) indítója</span><span class="sxs-lookup"><span data-stu-id="8768f-346">Launcher ERROR (Sqoop)</span></span>

<span data-ttu-id="8768f-347">**A jelenség**: hello túl a feladat állapotának megváltozásakor**KILLED**.</span><span class="sxs-lookup"><span data-stu-id="8768f-347">**Symptoms**: hello job status changes too**KILLED**.</span></span> <span data-ttu-id="8768f-348">Hello feladat részleteinek megjelenítése hello RunSqoopExport állapotának **hiba**.</span><span class="sxs-lookup"><span data-stu-id="8768f-348">Details for hello job show hello RunSqoopExport status as **ERROR**.</span></span> <span data-ttu-id="8768f-349">A következő hibaüzenet hello hello művelet kiválasztása jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="8768f-349">Selecting hello action shows hello following error message:</span></span>

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

<span data-ttu-id="8768f-350">**OK**: Sqoop értéke nem lehet tooload hello illesztőprogram szükséges tooaccess hello adatbázist.</span><span class="sxs-lookup"><span data-stu-id="8768f-350">**Cause**: Sqoop is unable tooload hello database driver required tooaccess hello database.</span></span>

<span data-ttu-id="8768f-351">**Megoldási**: Sqoop egy Oozie feladatból használatakor meg kell adni hello adatbázis illesztőprogram hello más erőforrások (például a hello workflow.xml) hello feladat használja.</span><span class="sxs-lookup"><span data-stu-id="8768f-351">**Resolution**: When using Sqoop from an Oozie job, you must include hello database driver with hello other resources (such as hello workflow.xml) used by hello job.</span></span> <span data-ttu-id="8768f-352">Hello archív hello adatbázis illesztőprogram hello tartalmazó is hivatkozni `<sqoop>...</sqoop>` hello workflow.xml szakasza.</span><span class="sxs-lookup"><span data-stu-id="8768f-352">Also Reference hello archive containing hello database driver from hello `<sqoop>...</sqoop>` section of hello workflow.xml.</span></span>

<span data-ttu-id="8768f-353">Például hello feladat ebben a dokumentumban, használja a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-353">For example, for hello job in this document, you would use hello following steps:</span></span>

1. <span data-ttu-id="8768f-354">Másolja a hello sqljdbc4.1.jar toohello /tutorials/useoozie könyvtára:</span><span class="sxs-lookup"><span data-stu-id="8768f-354">Copy hello sqljdbc4.1.jar file toohello /tutorials/useoozie directory:</span></span>

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. <span data-ttu-id="8768f-355">Módosítsa a hello workflow.xml tooadd hello XML a fenti új sor a következő `</sqoop>`:</span><span class="sxs-lookup"><span data-stu-id="8768f-355">Modify hello workflow.xml tooadd hello following XML on a new line above `</sqoop>`:</span></span>

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a><span data-ttu-id="8768f-356">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8768f-356">Next steps</span></span>

<span data-ttu-id="8768f-357">Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat és hogyan toorun egy Oozie feladat.</span><span class="sxs-lookup"><span data-stu-id="8768f-357">In this tutorial, you learned how toodefine an Oozie workflow and how toorun an Oozie job.</span></span> <span data-ttu-id="8768f-358">További információk a HDInsight, használata toolearn lásd: a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="8768f-358">toolearn more about working with HDInsight, see hello following articles:</span></span>

* <span data-ttu-id="8768f-359">[Időalapú Oozie-koordinátor használata a hdinsight eszközzel][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="8768f-359">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="8768f-360">[Hdinsight Hadoop-feladatokat az adatok feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="8768f-360">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="8768f-361">[Sqoop használata a hadooppal a Hdinsightban][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="8768f-361">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="8768f-362">[A Hive használata a hdinsight Hadoop][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="8768f-362">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="8768f-363">[A Pig használata a HDInsight Hadoop][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="8768f-363">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="8768f-364">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="8768f-364">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

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
