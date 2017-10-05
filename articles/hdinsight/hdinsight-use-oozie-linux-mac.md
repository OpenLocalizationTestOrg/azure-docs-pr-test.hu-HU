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
# <a name="use-oozie-with-hadoop-to-define-and-run-a-workflow-on-linux-based-hdinsight"></a><span data-ttu-id="cfd13-104">Határozza meg, és egy munkafolyamat futtatása a Linux-alapú HDInsight Hadoop Oozie használata</span><span class="sxs-lookup"><span data-stu-id="cfd13-104">Use Oozie with Hadoop to define and run a workflow on Linux-based HDInsight</span></span>

[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="cfd13-105">Útmutató Apache Oozie használata a HDInsight Hadoop.</span><span class="sxs-lookup"><span data-stu-id="cfd13-105">Learn how to use Apache Oozie with Hadoop on HDInsight.</span></span> <span data-ttu-id="cfd13-106">Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli.</span><span class="sxs-lookup"><span data-stu-id="cfd13-106">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="cfd13-107">Oozie integrálva van a Hadoop-veremmel, és támogatja a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-107">Oozie is integrated with the Hadoop stack, and it supports the following jobs:</span></span>

* <span data-ttu-id="cfd13-108">Apache MapReduce</span><span class="sxs-lookup"><span data-stu-id="cfd13-108">Apache MapReduce</span></span>
* <span data-ttu-id="cfd13-109">Apache Pig</span><span class="sxs-lookup"><span data-stu-id="cfd13-109">Apache Pig</span></span>
* <span data-ttu-id="cfd13-110">Apache Hive</span><span class="sxs-lookup"><span data-stu-id="cfd13-110">Apache Hive</span></span>
* <span data-ttu-id="cfd13-111">Apache Sqoop</span><span class="sxs-lookup"><span data-stu-id="cfd13-111">Apache Sqoop</span></span>

<span data-ttu-id="cfd13-112">Oozie is használható, például Java programok vagy héjparancsfájlok ütemezésére rendszerspecifikus feladatok ütemezése</span><span class="sxs-lookup"><span data-stu-id="cfd13-112">Oozie can also be used to schedule jobs that are specific to a system, like Java programs or shell scripts</span></span>

> [!NOTE]
> <span data-ttu-id="cfd13-113">A munkafolyamatok és a HDInsight együttes meghatározásához másik lehetőség is Azure Data Factory.</span><span class="sxs-lookup"><span data-stu-id="cfd13-113">Another option for defining workflows with HDInsight is Azure Data Factory.</span></span> <span data-ttu-id="cfd13-114">Azure Data Factory kapcsolatos további információkért lásd: [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="cfd13-114">To learn more about Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfd13-115">Oozie nincs engedélyezve a HDInsight-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-115">Oozie is not enabled on domain-joined HDInsight.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cfd13-116">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cfd13-116">Prerequisites</span></span>

* <span data-ttu-id="cfd13-117">**HDInsight-fürtök**: lásd: [HDInsight Linux első lépések](hdinsight-hadoop-linux-tutorial-get-started.md)</span><span class="sxs-lookup"><span data-stu-id="cfd13-117">**An HDInsight cluster**: See [Get Started with HDInsight on Linux](hdinsight-hadoop-linux-tutorial-get-started.md)</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="cfd13-118">A jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="cfd13-118">The steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="cfd13-119">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="cfd13-119">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="cfd13-120">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="cfd13-120">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="example-workflow"></a><span data-ttu-id="cfd13-121">Példa-munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="cfd13-121">Example workflow</span></span>

<span data-ttu-id="cfd13-122">A munkafolyamat a dokumentumban használt két műveleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-122">The workflow used in this document contains two actions.</span></span> <span data-ttu-id="cfd13-123">Műveletek végezhetők a definíciók feladatok, például a Hive, a Sqoop, a MapReduce vagy a másik folyamat fut:</span><span class="sxs-lookup"><span data-stu-id="cfd13-123">Actions are definitions for tasks, such as running Hive, Sqoop, MapReduce, or other process:</span></span>

![Munkafolyamat diagramja][img-workflow-diagram]

1. <span data-ttu-id="cfd13-125">A Hive művelet a futás bejegyzéseit kibontása a HiveQL-parancsfájlt a **hivesampletable** HDInsight része.</span><span class="sxs-lookup"><span data-stu-id="cfd13-125">A Hive action runs a HiveQL script to extract records from the **hivesampletable** included with HDInsight.</span></span> <span data-ttu-id="cfd13-126">Minden egyes soraiban levő adatok egy adott mobileszközről meglátogatása ismerteti.</span><span class="sxs-lookup"><span data-stu-id="cfd13-126">Each row of data describes a visit from a specific mobile device.</span></span> <span data-ttu-id="cfd13-127">A rekord formátumban jelenik meg az alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="cfd13-127">The record format appears similar to the following text:</span></span>

        8       18:54:20        en-US   Android Samsung SCH-i500        California     United States    13.9204007      0       0
        23      19:19:44        en-US   Android HTC     Incredible      Pennsylvania   United States    NULL    0       0
        23      19:19:46        en-US   Android HTC     Incredible      Pennsylvania   United States    1.4757422       0       1

    <span data-ttu-id="cfd13-128">Ebben a dokumentumban használt Hive-parancsfájl megjeleníti az egyes platformokon (például Android vagy iPhone) teljes látogatások, és a számát, az új Hive tábla tárolja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-128">The Hive script used in this document counts the total visits for each platform (such as Android or iPhone) and stores the counts to a new Hive table.</span></span>

    <span data-ttu-id="cfd13-129">További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="cfd13-129">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>

2. <span data-ttu-id="cfd13-130">A Sqoop művelet a tartalmát az új Hive tábla exportálása egy Azure SQL adatbázis egyik táblája.</span><span class="sxs-lookup"><span data-stu-id="cfd13-130">A Sqoop action exports the contents of the new Hive table to a table in an Azure SQL database.</span></span> <span data-ttu-id="cfd13-131">További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="cfd13-131">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="cfd13-132">Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított Hadoop-fürt verziók][hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="cfd13-132">For supported Oozie versions on HDInsight clusters, see [What's new in the Hadoop cluster versions provided by HDInsight][hdinsight-versions].</span></span>

## <a name="create-the-working-directory"></a><span data-ttu-id="cfd13-133">A munkakönyvtár létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfd13-133">Create the working directory</span></span>

<span data-ttu-id="cfd13-134">Oozie vár egy feladatot, amely ugyanabban a könyvtárban kell tárolni szükséges erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-134">Oozie expects resources required for a job to be stored in the same directory.</span></span> <span data-ttu-id="cfd13-135">Ez a példa **wasb: / / / oktatóanyagok/useoozie**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-135">This example uses **wasb:///tutorials/useoozie**.</span></span> <span data-ttu-id="cfd13-136">A következő paranccsal ez, és az adatok könyvtárat, amely tárolja az ezen munkafolyamat által létrehozott új Hive tábla létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="cfd13-136">Use the following command to create this directory, and the data directory that holds the new Hive table created by this workflow:</span></span>

```
hdfs dfs -mkdir -p /tutorials/useoozie/data
```

> [!NOTE]
> <span data-ttu-id="cfd13-137">A `-p` paraméter okoz az összes könyvtár elérési útját hozható létre.</span><span class="sxs-lookup"><span data-stu-id="cfd13-137">The `-p` parameter causes all directories in the path to be created.</span></span> <span data-ttu-id="cfd13-138">A **adatok** által használt adatok tárolására használt könyvtárat a **useooziewf.hql** parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="cfd13-138">The **data** directory is used to hold data used by the **useooziewf.hql** script.</span></span>

<span data-ttu-id="cfd13-139">Is futtassa a következő parancsot, amely biztosítja, hogy a Oozie megszemélyesíthet-e a felhasználói fiók Hive és a Sqoop feladat futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="cfd13-139">Also run the following command, which ensures that Oozie can impersonate your user account when running Hive and Sqoop jobs.</span></span> <span data-ttu-id="cfd13-140">Cserélje le **felhasználónév** a bejelentkezési névvel:</span><span class="sxs-lookup"><span data-stu-id="cfd13-140">Replace **USERNAME** with your login name:</span></span>

```
sudo adduser USERNAME users
```

> [!NOTE]
> <span data-ttu-id="cfd13-141">A hibákat, a felhasználó már tagja figyelmen kívül hagyhatja a `users` csoport.</span><span class="sxs-lookup"><span data-stu-id="cfd13-141">You can ignore errors that the user is already a member of the `users` group.</span></span>

## <a name="add-a-database-driver"></a><span data-ttu-id="cfd13-142">Egy adatbázis-illesztőprogram hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cfd13-142">Add a database driver</span></span>

<span data-ttu-id="cfd13-143">Mivel ez a munkafolyamat Sqoop használ az adatok exportálása az SQL Database, meg kell adnia egy példányát az SQL-adatbázis felvegye használt JDBC-illesztőt.</span><span class="sxs-lookup"><span data-stu-id="cfd13-143">Since this workflow uses Sqoop to export data to SQL Database, you must provide a copy of the JDBC driver used to talk to SQL Database.</span></span> <span data-ttu-id="cfd13-144">Az alábbi parancs segítségével másolja azt a munkakönyvtárat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-144">Use the following command to copy it to the working directory:</span></span>

```
hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc*.jar /tutorials/useoozie/
```

<span data-ttu-id="cfd13-145">Ha a munkafolyamat más erőforrások, például a MapReduce-alkalmazást tartalmazó jar használ, adja hozzá ezeket az erőforrásokat, valamint kellene.</span><span class="sxs-lookup"><span data-stu-id="cfd13-145">If your workflow used other resources, such as a jar containing a MapReduce application, you would need to add these resources as well.</span></span>

## <a name="define-the-hive-query"></a><span data-ttu-id="cfd13-146">A Hive-lekérdezés megadása</span><span class="sxs-lookup"><span data-stu-id="cfd13-146">Define the Hive query</span></span>

<span data-ttu-id="cfd13-147">A következő lépésekkel HiveQL-parancsfájlt, amely meghatározza az Oozie-munkafolyamat a dokumentum későbbi szakaszában a lekérdezés létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="cfd13-147">Use the following steps to create a HiveQL script that defines a query, which is used in an Oozie workflow later in this document.</span></span>

1. <span data-ttu-id="cfd13-148">Csatlakozzon a fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="cfd13-148">Connect to the cluster using SSH.</span></span> <span data-ttu-id="cfd13-149">A következő parancsot az használatának példája a `ssh` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cfd13-149">The following command is an example of using the `ssh` command.</span></span> <span data-ttu-id="cfd13-150">Cserélje le __felhasználónév__ a fürthöz az SSH felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="cfd13-150">Replace __USERNAME__ with the SSH user for the cluster.</span></span> <span data-ttu-id="cfd13-151">Cserélje le __CLUSTERNAME__ a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="cfd13-151">Replace __CLUSTERNAME__ with the name of the HDInsight cluster.</span></span>

    ```
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="cfd13-152">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="cfd13-152">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="cfd13-153">Az SSH-kapcsolat alkalmazás-fájl létrehozásához a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cfd13-153">From the SSH connection, use the following command to create a file:</span></span>

    ```
    nano useooziewf.hql
    ```

3. <span data-ttu-id="cfd13-154">Megnyílik a nano szerkesztő, ha a fájl tartalmát a következő lekérdezés használata:</span><span class="sxs-lookup"><span data-stu-id="cfd13-154">Once the nano editor opens, use the following query as the contents of the file:</span></span>

    ```hiveql
    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(deviceplatform string, count string) ROW FORMAT DELIMITED
    FIELDS TERMINATED BY '\t' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE TABLE ${hiveTableName} SELECT deviceplatform, COUNT(*) as count FROM hivesampletable GROUP BY deviceplatform;
    ```

    <span data-ttu-id="cfd13-155">Nincsenek a parancsfájl használt két változót:</span><span class="sxs-lookup"><span data-stu-id="cfd13-155">There are two variables used in the script:</span></span>

    * <span data-ttu-id="cfd13-156">**{hiveTableName} $**: hozható létre a tábla nevét tartalmazza</span><span class="sxs-lookup"><span data-stu-id="cfd13-156">**${hiveTableName}**: contains the name of the table to be created</span></span>

    * <span data-ttu-id="cfd13-157">**{hiveDataFolder} $**: a tábla az adatforrás adatfájljainak tárolási helyét tartalmazza</span><span class="sxs-lookup"><span data-stu-id="cfd13-157">**${hiveDataFolder}**: contains the location to store the data files for the table</span></span>

    <span data-ttu-id="cfd13-158">A munkafolyamat-definíciós fájl (ebben az oktatóanyagban workflow.xml) továbbítja ezeket az értékeket a HiveQL-parancsfájlt futás közben</span><span class="sxs-lookup"><span data-stu-id="cfd13-158">The workflow definition file (workflow.xml in this tutorial) passes these values to this HiveQL script at run time</span></span>

4. <span data-ttu-id="cfd13-159">Zárja be a szerkesztőt, nyomja le a Ctrl-X.</span><span class="sxs-lookup"><span data-stu-id="cfd13-159">To exit the editor, press Ctrl-X.</span></span> <span data-ttu-id="cfd13-160">Amikor a rendszer kéri, válassza ki a **Y** mentse a fájlt, majd használja **Enter** használata a **useooziewf.hql** fájl nevét.</span><span class="sxs-lookup"><span data-stu-id="cfd13-160">When prompted, select **Y** to save the file, then use **Enter** to use the **useooziewf.hql** file name.</span></span>

5. <span data-ttu-id="cfd13-161">Másolja az alábbi parancsokkal **useooziewf.hql** való **wasb:///tutorials/useoozie/useooziewf.hql**:</span><span class="sxs-lookup"><span data-stu-id="cfd13-161">Use the following commands to copy **useooziewf.hql** to **wasb:///tutorials/useoozie/useooziewf.hql**:</span></span>

    ```
    hdfs dfs -put useooziewf.hql /tutorials/useoozie/useooziewf.hql
    ```

    <span data-ttu-id="cfd13-162">Ezek a parancsok tárolja a **useooziewf.hql** a fürt a HDFS-kompatibilis tároló fájlt.</span><span class="sxs-lookup"><span data-stu-id="cfd13-162">These commands store the **useooziewf.hql** file on the HDFS-compatible storage for the cluster.</span></span>

## <a name="define-the-workflow"></a><span data-ttu-id="cfd13-163">Adja meg a munkafolyamat</span><span class="sxs-lookup"><span data-stu-id="cfd13-163">Define the workflow</span></span>

<span data-ttu-id="cfd13-164">Oozie munkafolyamatok definíciók hPDL (az XML folyamat Definition Language) nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="cfd13-164">Oozie workflows definitions are written in hPDL (an XML Process Definition Language).</span></span> <span data-ttu-id="cfd13-165">Az alábbi lépések segítségével határozza meg a munkafolyamat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-165">Use the following steps to define the workflow:</span></span>

1. <span data-ttu-id="cfd13-166">A következő utasítás használatával hozzon létre és szerkeszthet egy új fájlt:</span><span class="sxs-lookup"><span data-stu-id="cfd13-166">Use the following statement to create and edit a new file:</span></span>

    ```
    nano workflow.xml
    ```

2. <span data-ttu-id="cfd13-167">Megnyílik a nano szerkesztő, miután adja meg a következő XML fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="cfd13-167">Once the nano editor opens, enter the following XML as the file contents:</span></span>

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

    <span data-ttu-id="cfd13-168">A munkafolyamatban meghatározott két tevékenységek vannak:</span><span class="sxs-lookup"><span data-stu-id="cfd13-168">There are two actions defined in the workflow:</span></span>

   * <span data-ttu-id="cfd13-169">**RunHiveScript**: Ez a művelet az indítási műveletet, és futtatja a **useooziewf.hql** Hive parancsprogram</span><span class="sxs-lookup"><span data-stu-id="cfd13-169">**RunHiveScript**: This action is the start action, and runs the **useooziewf.hql** Hive script</span></span>

   * <span data-ttu-id="cfd13-170">**RunSqoopExport**: Ez a művelet exportálja az adatokat az SQL Database szolgáltatásba Sqoop a Hive-parancsfájl alapján létrehozott.</span><span class="sxs-lookup"><span data-stu-id="cfd13-170">**RunSqoopExport**: This action exports the data created from the Hive script to SQL Database using Sqoop.</span></span> <span data-ttu-id="cfd13-171">Ez a művelet csak akkor fut, ha a **RunHiveScript** művelet befejeződött.</span><span class="sxs-lookup"><span data-stu-id="cfd13-171">This action only runs if the **RunHiveScript** action is successful.</span></span>

     <span data-ttu-id="cfd13-172">A munkafolyamat rendelkezik több bejegyzést, például a `${jobTracker}`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-172">The workflow has several entries, such as `${jobTracker}`.</span></span> <span data-ttu-id="cfd13-173">Ezek a bejegyzések feladatdefinícióban használt értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="cfd13-173">These entries are replaced by values you use in the job definition.</span></span> <span data-ttu-id="cfd13-174">A feladat meghatározása a jelen dokumentum jön létre.</span><span class="sxs-lookup"><span data-stu-id="cfd13-174">The job definition is created later in this document.</span></span>

     <span data-ttu-id="cfd13-175">Azt is vegye figyelembe a `<archive>sqljdbc4.jar</arcive>` a Sqoop szakasz bejegyzése.</span><span class="sxs-lookup"><span data-stu-id="cfd13-175">Also note the `<archive>sqljdbc4.jar</arcive>` entry in the Sqoop section.</span></span> <span data-ttu-id="cfd13-176">Ez a bejegyzés arra utasítja az Oozie elérhetővé ebből az archívumból Sqoop számára ez a művelet futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="cfd13-176">This entry instructs Oozie to make this archive available for Sqoop when this action runs.</span></span>

3. <span data-ttu-id="cfd13-177">Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="cfd13-177">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

4. <span data-ttu-id="cfd13-178">Másolja az alábbi parancs segítségével a **workflow.xml** fájl **/tutorials/useoozie/workflow.xml**:</span><span class="sxs-lookup"><span data-stu-id="cfd13-178">Use the following command to copy the **workflow.xml** file to **/tutorials/useoozie/workflow.xml**:</span></span>

    ```
    hdfs dfs -put workflow.xml /tutorials/useoozie/workflow.xml
    ```

## <a name="create-the-database"></a><span data-ttu-id="cfd13-179">Az adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfd13-179">Create the database</span></span>

<span data-ttu-id="cfd13-180">Az Azure SQL adatbázis létrehozásához kövesse a [SQL-adatbázis létrehozása](../sql-database/sql-database-get-started.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cfd13-180">To create an Azure SQL Database, follow the steps in the [Create a SQL Database](../sql-database/sql-database-get-started.md) document.</span></span> <span data-ttu-id="cfd13-181">Az adatbázis létrehozásakor `oozietest` az adatbázis neve.</span><span class="sxs-lookup"><span data-stu-id="cfd13-181">When creating the database, use `oozietest` as the database name.</span></span> <span data-ttu-id="cfd13-182">Is jegyezze fel az adatbázis-kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="cfd13-182">Also make a note of the name of the database server.</span></span>

### <a name="create-the-table"></a><span data-ttu-id="cfd13-183">A tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfd13-183">Create the table</span></span>

> [!NOTE]
> <span data-ttu-id="cfd13-184">Számos módon hozzon létre egy táblát az SQL-adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-184">There are many ways to connect to SQL Database to create a table.</span></span> <span data-ttu-id="cfd13-185">Az alábbi lépéseket használata [FreeTDS](http://www.freetds.org/) a a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-185">The following steps use [FreeTDS](http://www.freetds.org/) from the HDInsight cluster.</span></span>


1. <span data-ttu-id="cfd13-186">A következő paranccsal FreeTDS a HDInsight-fürt telepítése:</span><span class="sxs-lookup"><span data-stu-id="cfd13-186">Use the following command to install FreeTDS on the HDInsight cluster:</span></span>

    ```
    sudo apt-get --assume-yes install freetds-dev freetds-bin
    ```

2. <span data-ttu-id="cfd13-187">Egyszer FreeTDS telepítve van, ezért a korábban létrehozott SQL Database-kiszolgálóhoz való kapcsolódáshoz a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cfd13-187">Once FreeTDS has been installed, use the following command to connect to the SQL Database server you created previously:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <sqlLogin> -P <sqlPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="cfd13-188">A kimenet az alábbihoz hasonló jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="cfd13-188">You receive output similar to the following text:</span></span>

        locale is "en_US.UTF-8"
        locale charset is "UTF-8"
        using default charset "UTF-8"
        Default database being set to oozietest
        1>

3. <span data-ttu-id="cfd13-189">: A `1>` kéri, adja meg a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-189">At the `1>` prompt, enter the following lines:</span></span>

    ```
    CREATE TABLE [dbo].[mobiledata](
    [deviceplatform] [nvarchar](50),
    [count] [bigint])
    GO
    CREATE CLUSTERED INDEX mobiledata_clustered_index on mobiledata(deviceplatform)
    GO
    ```

    <span data-ttu-id="cfd13-190">Ha a `GO` utasításban is meg kell adni, az előző utasítások kiértékelése.</span><span class="sxs-lookup"><span data-stu-id="cfd13-190">When the `GO` statement is entered, the previous statements are evaluated.</span></span> <span data-ttu-id="cfd13-191">Ezekre az utasításokra, hozzon létre egy táblát nevű **mobiledata** a munkafolyamat által használt.</span><span class="sxs-lookup"><span data-stu-id="cfd13-191">These statements create a table named **mobiledata** that is used by the workflow.</span></span>

    <span data-ttu-id="cfd13-192">A következő segítségével győződjön meg arról, hogy a tábla jött létre:</span><span class="sxs-lookup"><span data-stu-id="cfd13-192">Use the following to verify that the table has been created:</span></span>

    ```
    SELECT * FROM information_schema.tables
    GO
    ```

    <span data-ttu-id="cfd13-193">A kimenet az alábbihoz hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cfd13-193">You see output similar to the following text:</span></span>

    ```
    TABLE_CATALOG   TABLE_SCHEMA    TABLE_NAME      TABLE_TYPE
    oozietest       dbo     mobiledata      BASE TABLE
    ```

4. <span data-ttu-id="cfd13-194">Adja meg `exit` , a `1>` Rákérdezés a tsql segédprogram kilép.</span><span class="sxs-lookup"><span data-stu-id="cfd13-194">Enter `exit` at the `1>` prompt to exit the tsql utility.</span></span>

## <a name="create-the-job-definition"></a><span data-ttu-id="cfd13-195">A feladat definíciójának létrehozása</span><span class="sxs-lookup"><span data-stu-id="cfd13-195">Create the job definition</span></span>

<span data-ttu-id="cfd13-196">A feladat definíciójához hol található a workflow.xml ismerteti.</span><span class="sxs-lookup"><span data-stu-id="cfd13-196">The job definition describes where to find the workflow.xml.</span></span> <span data-ttu-id="cfd13-197">Azt is ismerteti, hol található egyéb fájlok (például useooziewf.hql.) a munkafolyamat által használt Azt is meghatározza az értékek tulajdonságok a munkafolyamaton belül használt, és a kapcsolódó fájlokat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-197">It also describes where to find other files used by the workflow (such as useooziewf.hql.) It also defines the values for properties used within the workflow and associated files.</span></span>

1. <span data-ttu-id="cfd13-198">A következő paranccsal alapértelmezett tárterület teljes címét.</span><span class="sxs-lookup"><span data-stu-id="cfd13-198">Use the following command to get the full address of the default storage.</span></span> <span data-ttu-id="cfd13-199">Ez a cím néhány percet szerepel a konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="cfd13-199">This address is used in the configuration file in a moment:</span></span>

    ```
    sed -n '/<name>fs.default/,/<\/value>/p' /etc/hadoop/conf/core-site.xml
    ```

    <span data-ttu-id="cfd13-200">Ez a parancs információkat ad vissza a következő XML-hasonló:</span><span class="sxs-lookup"><span data-stu-id="cfd13-200">This command returns information similar to the following XML:</span></span>

    ```xml
    <name>fs.defaultFS</name>
    <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net</value>
    ```

    > [!NOTE]
    > <span data-ttu-id="cfd13-201">Ha a HDInsight-fürt Azure a tárolót használja az alapértelmezett tárolóként a `<value>` elem tartalma kezdődnie `wasb://`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-201">If the HDInsight cluster uses Azure Storage as the default storage, the `<value>` element contents begin with `wasb://`.</span></span> <span data-ttu-id="cfd13-202">Ha az Azure Data Lake Store használata esetén kezdődik `adl://`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-202">If Azure Data Lake Store is used instead, it begins with `adl://`.</span></span>

    <span data-ttu-id="cfd13-203">Menti a `<value>` elem, mert használatban van a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cfd13-203">Save the contents of the `<value>` element, as it is used in the next steps.</span></span>

2. <span data-ttu-id="cfd13-204">A következő paranccsal lekérni a fürt headnode teljes Tartományneve.</span><span class="sxs-lookup"><span data-stu-id="cfd13-204">Use the following command to get FQDN of the cluster headnode.</span></span> <span data-ttu-id="cfd13-205">Ezt az információt a Hadoopból címet a fürtnek szolgál:</span><span class="sxs-lookup"><span data-stu-id="cfd13-205">This information is used for the JobTracker address for the cluster:</span></span>

    ```
    hostname -f
    ```

    <span data-ttu-id="cfd13-206">Ez hasonló ad vissza adatokat a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfd13-206">This returns information similar to the following text:</span></span>

    ```hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net```

    <span data-ttu-id="cfd13-207">A Hadoopból használt port 8050,, így a Hadoopból használt a teljes címet van `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-207">The port used for the JobTracker is 8050, so the full address to use for the JobTracker is `hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8050`.</span></span>

3. <span data-ttu-id="cfd13-208">Az Oozie feladat definition konfiguráció létrehozásához használja a következőket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-208">Use the following to create the Oozie job definition configuration:</span></span>

    ```
    nano job.xml
    ```

4. <span data-ttu-id="cfd13-209">Amikor megnyílik a nano szerkesztő, használja a következő XML, a fájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="cfd13-209">Once the nano editor opens, use the following XML as the contents of the file:</span></span>

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

   * <span data-ttu-id="cfd13-210">Cserélje le az összes példányát  **wasb://mycontainer@mystorageaccount.blob.core.windows.net**  alapértelmezett tárolási korábban fogadott értékkel.</span><span class="sxs-lookup"><span data-stu-id="cfd13-210">Replace all instances of **wasb://mycontainer@mystorageaccount.blob.core.windows.net** with the value you received earlier for default storage.</span></span>

     > [!WARNING]
     > <span data-ttu-id="cfd13-211">Ha az elérési út egy `wasb` elérési útja, a teljes elérési útja kell használnia.</span><span class="sxs-lookup"><span data-stu-id="cfd13-211">If the path is a `wasb` path, you must use the full path.</span></span> <span data-ttu-id="cfd13-212">Nem rövidíthető úgy, hogy csak `wasb:///`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-212">Do not shorten it to just `wasb:///`.</span></span>

   * <span data-ttu-id="cfd13-213">Cserélje le **JOBTRACKERADDRESS** korábban fogadott JobTracker/ResourceManager címmel.</span><span class="sxs-lookup"><span data-stu-id="cfd13-213">Replace **JOBTRACKERADDRESS** with the JobTracker/ResourceManager address you received earlier.</span></span>
   * <span data-ttu-id="cfd13-214">Cserélje le **Saját_név** a bejelentkezési névvel a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-214">Replace **YourName** with your login name for the HDInsight cluster.</span></span>
   * <span data-ttu-id="cfd13-215">Cserélje le **kiszolgálónév**, **adminLogin**, és **adminPassword** és az Azure SQL Database vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-215">Replace **serverName**, **adminLogin**, and **adminPassword** with the information for your Azure SQL Database.</span></span>

     <span data-ttu-id="cfd13-216">A legtöbb a fájlban található információ a workflow.xml vagy ooziewf.hql fájlokat (például ${nameNode}.) a használt értékek feltöltésére használt eszköz</span><span class="sxs-lookup"><span data-stu-id="cfd13-216">Most of the information in this file is used to populate the values used in the workflow.xml or ooziewf.hql files (such as ${nameNode}.)</span></span>

     > [!NOTE]
     > <span data-ttu-id="cfd13-217">A **oozie.wf.application.path** a bejegyzés határozza meg, hol található a workflow.xml fájl, a feladat futott, amely tartalmazza a munkafolyamat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-217">The **oozie.wf.application.path** entry defines where to find the workflow.xml file, which contains the workflow ran by this job.</span></span>

5. <span data-ttu-id="cfd13-218">Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="cfd13-218">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

## <a name="submit-and-manage-the-job"></a><span data-ttu-id="cfd13-219">Küldje el, majd a feladatok kezelése</span><span class="sxs-lookup"><span data-stu-id="cfd13-219">Submit and manage the job</span></span>

<span data-ttu-id="cfd13-220">Az alábbi lépéseket az Oozie-parancs segítségével küldje el, majd a fürt Oozie munkafolyamatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="cfd13-220">The following steps use the Oozie command to submit and manage Oozie workflows on the cluster.</span></span> <span data-ttu-id="cfd13-221">A Oozie parancs nem egy egyszerű felületen keresztül a [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="cfd13-221">The Oozie command is a friendly interface over the [Oozie REST API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cfd13-222">Az Oozie-paranccsal, ha a HDInsight headnode a teljes Tartománynevét kell használnia.</span><span class="sxs-lookup"><span data-stu-id="cfd13-222">When using the Oozie command, you must use the FQDN for the HDInsight headnode.</span></span> <span data-ttu-id="cfd13-223">Ez a teljes tartománynév csak az elérhető a fürtből, vagy ha a fürt egy Azure virtuális hálózat ugyanazon a hálózaton lévő más gépekkel.</span><span class="sxs-lookup"><span data-stu-id="cfd13-223">This FQDN is only accessible from the cluster, or if the cluster is on an Azure Virtual Network, from other machines on the same network.</span></span>


1. <span data-ttu-id="cfd13-224">Használja a következő beszerzése az Oozie-szolgáltatás URL-címe:</span><span class="sxs-lookup"><span data-stu-id="cfd13-224">Use the following to obtain the URL to the Oozie service:</span></span>

    ```
    sed -n '/<name>oozie.base.url/,/<\/value>/p' /etc/oozie/conf/oozie-site.xml
    ```

    <span data-ttu-id="cfd13-225">Ez adatait adja vissza. a következő XML-hasonló:</span><span class="sxs-lookup"><span data-stu-id="cfd13-225">This returns information similar to the following XML:</span></span>

    ```xml
    <name>oozie.base.url</name>
    <value>http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie</value>
    ```

    <span data-ttu-id="cfd13-226">A `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` részére az URL-CÍMÉT az Oozie paranccsal.</span><span class="sxs-lookup"><span data-stu-id="cfd13-226">The `http://hn0-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:11000/oozie` portion is the URL to use with the Oozie command.</span></span>

2. <span data-ttu-id="cfd13-227">Használja a következő környezeti változó létrehozása az URL-cím, így nem kell minden parancs típusról:</span><span class="sxs-lookup"><span data-stu-id="cfd13-227">Use the following to create an environment variable for the URL, so you don't have to type it for every command:</span></span>

    ```
    export OOZIE_URL=http://HOSTNAMEt:11000/oozie
    ```

    <span data-ttu-id="cfd13-228">Cserélje le a korábban kapott egy URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="cfd13-228">Replace the URL with the one you received earlier.</span></span>
3. <span data-ttu-id="cfd13-229">A feladat elküldéséhez használja a következő:</span><span class="sxs-lookup"><span data-stu-id="cfd13-229">Use the following to submit the job:</span></span>

    ```
    oozie job -config job.xml -submit
    ```

    <span data-ttu-id="cfd13-230">Ez a parancs betölti a feladat adatait **job.xml** , és elküldi Oozie, de nem futtatja a programot.</span><span class="sxs-lookup"><span data-stu-id="cfd13-230">This command loads the job information from **job.xml** and submits it to Oozie, but does not run it.</span></span>

    <span data-ttu-id="cfd13-231">A parancs után ez a feladat Azonosítóját kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="cfd13-231">Once the command completes, it should return the ID of the job.</span></span> <span data-ttu-id="cfd13-232">Például: `0000005-150622124850154-oozie-oozi-W`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-232">For example, `0000005-150622124850154-oozie-oozi-W`.</span></span> <span data-ttu-id="cfd13-233">Ezt az Azonosítót a feladat kezelésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="cfd13-233">This ID is used to manage the job.</span></span>

4. <span data-ttu-id="cfd13-234">A következő parancsot a feladat állapotának megtekintése:</span><span class="sxs-lookup"><span data-stu-id="cfd13-234">View the status of the job using the following command:</span></span>

    ```
    oozie job -info <JOBID>
    ```

    > [!NOTE]
    > <span data-ttu-id="cfd13-235">Cserélje le `<JOBID>` az előző lépés eredményeképpen visszakapott azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cfd13-235">Replace `<JOBID>` with the ID returned in the previous step.</span></span>

    <span data-ttu-id="cfd13-236">Ez hasonló ad vissza adatokat a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfd13-236">This returns information similar to the following text:</span></span>

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

    <span data-ttu-id="cfd13-237">Ez a feladat állapota `PREP`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-237">This job has a status of `PREP`.</span></span> <span data-ttu-id="cfd13-238">Ez az állapot azt jelzi, hogy a feladat lett létrehozva, de nem lett elindítva.</span><span class="sxs-lookup"><span data-stu-id="cfd13-238">This status indicates that the job was created, but not started.</span></span>

5. <span data-ttu-id="cfd13-239">Az alábbi parancs segítségével indítsa el a feladatot:</span><span class="sxs-lookup"><span data-stu-id="cfd13-239">Use the following command to start the job:</span></span>

    ```
    oozie job -start JOBID
    ```

    > [!NOTE]
    > <span data-ttu-id="cfd13-240">Cserélje le `<JOBID>` korábban visszaadott azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="cfd13-240">Replace `<JOBID>` with the ID returned previously.</span></span>

    <span data-ttu-id="cfd13-241">Ez a parancs után ellenőrizze az állapot, ha egy futó állapotban van, és információk jelennek-e a feladat a műveletekhez.</span><span class="sxs-lookup"><span data-stu-id="cfd13-241">If you check the status after this command, it is in a running state, and information is returned for the actions within the job.</span></span>

6. <span data-ttu-id="cfd13-242">A feladat sikeres befejeződése után ellenőrizheti, hogy az adatok jön létre, és az SQL-adatbázis táblája exportált a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="cfd13-242">Once the task completes successfully, you can verify that the data was generated and exported to the SQL Database table by using the following commands:</span></span>

    ```
    TDSVER=8.0 tsql -H <serverName>.database.windows.net -U <adminLogin> -P <adminPassword> -p 1433 -D oozietest
    ```

    <span data-ttu-id="cfd13-243">: A `1>` kéri, adja meg a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="cfd13-243">At the `1>` prompt, enter the following query:</span></span>

    ```
    SELECT * FROM mobiledata
    GO
    ```

    <span data-ttu-id="cfd13-244">A visszakapott információk hasonlít a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="cfd13-244">The information returned is similar to the following text:</span></span>

        deviceplatform  count
        Android 31591
        iPhone OS       22731
        proprietary development 3
        RIM OS  3464
        Unknown 213
        Windows Phone   1791
        (6 rows affected)

<span data-ttu-id="cfd13-245">A Oozie parancs további információkért lásd: [Oozie parancssori eszköz](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span><span class="sxs-lookup"><span data-stu-id="cfd13-245">For more information on the Oozie command, see [Oozie command-line tool](https://oozie.apache.org/docs/4.1.0/DG_CommandLineTool.html).</span></span>

## <a name="oozie-rest-api"></a><span data-ttu-id="cfd13-246">Oozie REST API-n</span><span class="sxs-lookup"><span data-stu-id="cfd13-246">Oozie REST API</span></span>

<span data-ttu-id="cfd13-247">Az Oozie REST API lehetővé teszi a saját buildet Oozie együtt használható.</span><span class="sxs-lookup"><span data-stu-id="cfd13-247">The Oozie REST API allows you to build your own tools that work with Oozie.</span></span> <span data-ttu-id="cfd13-248">HDInsight Oozie REST API használatával kapcsolatos információk a következők:</span><span class="sxs-lookup"><span data-stu-id="cfd13-248">The following are HDInsight specific information about using the Oozie REST API:</span></span>

* <span data-ttu-id="cfd13-249">**URI**: A REST API érhetők el, a fürtön kívüli`https://CLUSTERNAME.azurehdinsight.net/oozie`</span><span class="sxs-lookup"><span data-stu-id="cfd13-249">**URI**: The REST API can be accessed from outside the cluster at `https://CLUSTERNAME.azurehdinsight.net/oozie`</span></span>

* <span data-ttu-id="cfd13-250">**Hitelesítési**: az a fürt HTTP-fiókkal (rendszergazda) és a jelszó API hitelesítését.</span><span class="sxs-lookup"><span data-stu-id="cfd13-250">**Authentication**: Authenticate to the API using the cluster HTTP account (admin) and password.</span></span> <span data-ttu-id="cfd13-251">Példa:</span><span class="sxs-lookup"><span data-stu-id="cfd13-251">For example:</span></span>

    ```
    curl -u admin:PASSWORD https://CLUSTERNAME.azurehdinsight.net/oozie/versions
    ```

<span data-ttu-id="cfd13-252">További információ az Oozie REST API használatával, lásd: [Oozie webszolgáltatási API-ra](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span><span class="sxs-lookup"><span data-stu-id="cfd13-252">For more information on using the Oozie REST API, see [Oozie Web Services API](https://oozie.apache.org/docs/4.1.0/WebServicesAPI.html).</span></span>

## <a name="oozie-web-ui"></a><span data-ttu-id="cfd13-253">Oozie webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="cfd13-253">Oozie Web UI</span></span>

<span data-ttu-id="cfd13-254">A Oozie webes felhasználói felület révén Oozie feladatok állapotának webes képet a fürtön.</span><span class="sxs-lookup"><span data-stu-id="cfd13-254">The Oozie Web UI provides a web-based view into the status of Oozie jobs on the cluster.</span></span> <span data-ttu-id="cfd13-255">A webes felhasználói felület lehetővé teszi a következőket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-255">The web UI allows you to view the following information:</span></span>

* <span data-ttu-id="cfd13-256">Feladat állapota</span><span class="sxs-lookup"><span data-stu-id="cfd13-256">Job status</span></span>
* <span data-ttu-id="cfd13-257">Feladatdefiníció</span><span class="sxs-lookup"><span data-stu-id="cfd13-257">Job definition</span></span>
* <span data-ttu-id="cfd13-258">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="cfd13-258">Configuration</span></span>
* <span data-ttu-id="cfd13-259">A műveletek a feladat grafikonja</span><span class="sxs-lookup"><span data-stu-id="cfd13-259">A graph of the actions in the job</span></span>
* <span data-ttu-id="cfd13-260">A feladat naplók</span><span class="sxs-lookup"><span data-stu-id="cfd13-260">Logs for the job</span></span>

<span data-ttu-id="cfd13-261">Műveletek a feladat részleteit is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="cfd13-261">You can also view details for actions within a job.</span></span>

<span data-ttu-id="cfd13-262">Szeretne használni az Oozie webes felhasználói felületén, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-262">To access the Oozie Web UI, use the following steps:</span></span>

1. <span data-ttu-id="cfd13-263">A HDInsight-fürthöz SSH-alagút létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cfd13-263">Create an SSH tunnel to the HDInsight cluster.</span></span> <span data-ttu-id="cfd13-264">További információ: a [használata SSH Tunneling hdinsight](hdinsight-linux-ambari-ssh-tunnel.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="cfd13-264">For information, see the [Use SSH Tunneling with HDInsight](hdinsight-linux-ambari-ssh-tunnel.md) document.</span></span>

2. <span data-ttu-id="cfd13-265">Ha alagút létrejött, nyissa meg az Ambari webes felhasználói felület a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="cfd13-265">Once a tunnel has been created, open the Ambari web UI in your web browser.</span></span> <span data-ttu-id="cfd13-266">Az URI-azonosítója az Ambari hely **https://CLUSTERNAME.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-266">The URI for the Ambari site is **https://CLUSTERNAME.azurehdinsight.net**.</span></span> <span data-ttu-id="cfd13-267">Cserélje le **CLUSTERNAME** nevű, a Linux-alapú HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-267">Replace **CLUSTERNAME** with the name of your Linux-based HDInsight cluster.</span></span>

3. <span data-ttu-id="cfd13-268">Válassza ki a lap bal oldalán, **Oozie**, majd **Gyorshivatkozások**, és végül **Oozie webes felhasználói felületén**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-268">From the left side of the page, select **Oozie**, then **Quick Links**, and finally **Oozie Web UI**.</span></span>

    ![a menük képe](./media/hdinsight-use-oozie-linux-mac/ooziewebuisteps.png)

4. <span data-ttu-id="cfd13-270">Az Oozie webes felhasználói felületének alapértelmezett futó munkafolyamat-feladatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="cfd13-270">The Oozie Web UI defaults to displaying running Workflow Jobs.</span></span> <span data-ttu-id="cfd13-271">Minden munkafolyamat-feladatok megtekintéséhez válasszon **összes feladat**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-271">To see all workflow jobs, select **All Jobs**.</span></span>

    ![Minden feladat jelenik meg](./media/hdinsight-use-oozie-linux-mac/ooziejobs.png)

5. <span data-ttu-id="cfd13-273">Jelölje ki a feladatot a feladattal kapcsolatos további információk megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="cfd13-273">Select a job to view more information about the job.</span></span>

    ![Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/jobinfo.png)

6. <span data-ttu-id="cfd13-275">A Feladatinformáció lapján tekintheti meg alapvető feladatinformációkat és az egyes műveletek a feladatban található.</span><span class="sxs-lookup"><span data-stu-id="cfd13-275">From the Job Info tab, you can see basic job information and the individual actions within the job.</span></span> <span data-ttu-id="cfd13-276">Használja a füleket felső megtekintheti a feladatdefiníció, feladat konfigurációs hozzáférés a Job Log vagy egy irányított aciklikus diagramhoz (DAG) a feladat megtekintése.</span><span class="sxs-lookup"><span data-stu-id="cfd13-276">Using the tabs at the top you can view the Job Definition, Job Configuration, access the Job Log or view a Directed Acyclic Graph (DAG) of the job.</span></span>

   * <span data-ttu-id="cfd13-277">**Job Log**: válassza ki a **GetLogs** gombra kattint, hogy a feladat összes naplójának beolvasása, vagy használja a **keresési szűrőt adjon meg** mező naplók szűrése</span><span class="sxs-lookup"><span data-stu-id="cfd13-277">**Job Log**: Select the **GetLogs** button to get all logs for the job, or use the **Enter Search Filter** field to filter logs</span></span>

       ![Feladat-napló](./media/hdinsight-use-oozie-linux-mac/joblog.png)

   * <span data-ttu-id="cfd13-279">**JobDAG**: az adatbázis-elérhetőségi csoport az adatelérési utak venni a munkafolyamaton keresztül egy grafikus áttekintése</span><span class="sxs-lookup"><span data-stu-id="cfd13-279">**JobDAG**: The DAG is a graphical overview of the data paths taken through the workflow</span></span>

       ![Feladat DAG](./media/hdinsight-use-oozie-linux-mac/jobdag.png)

7. <span data-ttu-id="cfd13-281">A műveletek egyikének kiválasztásával a **Feladatinformáció** információk a művelet lapon megjelenik.</span><span class="sxs-lookup"><span data-stu-id="cfd13-281">Selecting one of the actions from the **Job Info** tab brings up information for the action.</span></span> <span data-ttu-id="cfd13-282">Válassza például a **RunHiveScript** művelet.</span><span class="sxs-lookup"><span data-stu-id="cfd13-282">For example, select the **RunHiveScript** action.</span></span>

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/action.png)

8. <span data-ttu-id="cfd13-284">Megtekintheti a részleteket a művelet, például egy hivatkozást a **konzol URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-284">You can see details for the action, such as a link to the **Console URL**.</span></span> <span data-ttu-id="cfd13-285">Ez a hivatkozás segítségével JobTracker tekintse meg a feladathoz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-285">This link can be used to view JobTracker information for the job.</span></span>

## <a name="scheduling-jobs"></a><span data-ttu-id="cfd13-286">Feladatütemezés</span><span class="sxs-lookup"><span data-stu-id="cfd13-286">Scheduling jobs</span></span>

<span data-ttu-id="cfd13-287">A koordinátor lehetővé teszi, hogy adja meg egy kezdő, záró és feladatok előfordulási gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="cfd13-287">The coordinator allows you to specify a start, end, and occurrence frequency for jobs.</span></span> <span data-ttu-id="cfd13-288">A munkafolyamat ütemezését megadásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-288">To define a schedule for the workflow, use the following steps:</span></span>

1. <span data-ttu-id="cfd13-289">Használja a következő nevű fájl létrehozása **coordinator.xml**:</span><span class="sxs-lookup"><span data-stu-id="cfd13-289">Use the following to create a file named **coordinator.xml**:</span></span>

    ```
    nano coordinator.xml
    ```

    <span data-ttu-id="cfd13-290">A fájl tartalmát a következő XML-kód használata:</span><span class="sxs-lookup"><span data-stu-id="cfd13-290">Use the following XML as the contents of the file:</span></span>

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
    > <span data-ttu-id="cfd13-291">A `${...}` változók futásidőben feladatdefinícióban értékek helyett.</span><span class="sxs-lookup"><span data-stu-id="cfd13-291">The `${...}` variables are replaced by values in the job definition at run-time.</span></span> <span data-ttu-id="cfd13-292">A változók az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="cfd13-292">The variables are:</span></span>
    >
    > * <span data-ttu-id="cfd13-293">`${coordFrequency}`: A feladat példányt futtatva között eltelt idő.</span><span class="sxs-lookup"><span data-stu-id="cfd13-293">`${coordFrequency}`: Time between running instances of the job.</span></span>
    > <span data-ttu-id="cfd13-294">** `${coordStart}`: A feladat kezdési időpontja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-294">** `${coordStart}`: The job start time.</span></span>
    > * <span data-ttu-id="cfd13-295">`${coordEnd}`: A feladat befejezési időpontja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-295">`${coordEnd}`: The job end time.</span></span>
    > * <span data-ttu-id="cfd13-296">`${coordTimezone}`: Koordinátor feladatok vannak rögzített időzónában nem nyári időszámításra (általában képviseli UTC használatával).</span><span class="sxs-lookup"><span data-stu-id="cfd13-296">`${coordTimezone}`: Coordinator jobs are in a fixed time zone with no daylight savings time (typically represented by using UTC).</span></span> <span data-ttu-id="cfd13-297">Ez az időzóna kezeli a "Oozie feldolgozási időzónában."</span><span class="sxs-lookup"><span data-stu-id="cfd13-297">This time zone is referred as the "Oozie processing timezone."</span></span>
    > * <span data-ttu-id="cfd13-298">`${wfPath}`: Az a workflow.xml elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-298">`${wfPath}`: The path to the workflow.xml.</span></span>

2. <span data-ttu-id="cfd13-299">Mentse a fájlt, használja a Ctrl-X, **Y**, és **Enter**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-299">To save the file, use Ctrl-X, **Y**, and **Enter**.</span></span>

3. <span data-ttu-id="cfd13-300">Az alábbi parancs segítségével másolja a fájlt a feladat munkakönyvtára:</span><span class="sxs-lookup"><span data-stu-id="cfd13-300">Use the following command to copy the file to the working directory for this job:</span></span>

    ```
    hadoop fs -put coordinator.xml /tutorials/useoozie/coordinator.xml
    ```

4. <span data-ttu-id="cfd13-301">A következő használatával módosíthatja a **job.xml** fájlt:</span><span class="sxs-lookup"><span data-stu-id="cfd13-301">Use the following to modify the **job.xml** file:</span></span>

    ```
    nano job.xml
    ```

    <span data-ttu-id="cfd13-302">A következő módosításokat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-302">Make the following changes:</span></span>

   * <span data-ttu-id="cfd13-303">Kérje meg a koordinátor-fájl helyett a munkafolyamat futtatásához oozie, módosítsa `<name>oozie.wf.application.path</name>` való `<name>oozie.coord.application.path</name>`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-303">To instruct oozie to run the coordinator file instead of the workflow, change `<name>oozie.wf.application.path</name>` to `<name>oozie.coord.application.path</name>`.</span></span>

   * <span data-ttu-id="cfd13-304">Beállítása a `workflowPath` használják a koordinátor adja hozzá a következő XML változó:</span><span class="sxs-lookup"><span data-stu-id="cfd13-304">To set the `workflowPath` variable used by the coordinator, add the following XML:</span></span>

        ```xml
        <property>
            <name>workflowPath</name>
            <value>wasb://mycontainer@mystorageaccount.blob.core.windows.net/tutorials/useoozie</value>
        </property>
        ```

       <span data-ttu-id="cfd13-305">Cserélje le a `wasb://mycontainer@mystorageaccount.blob.core.windows` más bejegyzések a job.xml fájlban megadott értéknek szöveget.</span><span class="sxs-lookup"><span data-stu-id="cfd13-305">Replace the `wasb://mycontainer@mystorageaccount.blob.core.windows` text with the value used in other entries in the job.xml file.</span></span>

   * <span data-ttu-id="cfd13-306">A start, végfelhasználók és a koordinátor gyakorisága megadásához adja hozzá a következő XML:</span><span class="sxs-lookup"><span data-stu-id="cfd13-306">To define the start, end, and frequency for the coordinator, add the following XML:</span></span>

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

       <span data-ttu-id="cfd13-307">Ezek az értékek értékre van állítva a kezdési időpont 12:00 PM előfordulhat, hogy 10, 2017, 2017. május 12. a befejezési idő.</span><span class="sxs-lookup"><span data-stu-id="cfd13-307">These values set the start time to 12:00PM on May 10, 2017, the end time to May 12, 2017.</span></span> <span data-ttu-id="cfd13-308">Ez a feladat futtatása naponta intervallumát.</span><span class="sxs-lookup"><span data-stu-id="cfd13-308">The interval for running this job daily.</span></span> <span data-ttu-id="cfd13-309">A gyakoriság értéke (percben), így 24 óra x 60 perc = 1440 perc.</span><span class="sxs-lookup"><span data-stu-id="cfd13-309">The frequency is in minutes, so 24 hours x 60 minutes = 1440 minutes.</span></span> <span data-ttu-id="cfd13-310">Végezetül UTC időzóna van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cfd13-310">Finally, the timezone is set to UTC.</span></span>

5. <span data-ttu-id="cfd13-311">Használja a Ctrl-X, majd **Y** és **Enter** fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="cfd13-311">Use Ctrl-X, then **Y** and **Enter** to save the file.</span></span>

6. <span data-ttu-id="cfd13-312">A feladat futtatásához a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cfd13-312">To run the job, use the following command:</span></span>

    ```
    oozie job -config job.xml -run
    ```

    <span data-ttu-id="cfd13-313">Ez a parancs küldi el, és elindítja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="cfd13-313">This command submits and starts the job.</span></span>

7. <span data-ttu-id="cfd13-314">Ha látogasson el a Oozie webes felhasználói Felületet, és válassza a **koordinátor feladatok** lapon megjelenik az alábbi képen hasonló adatokat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-314">If you visit the Oozie Web UI and select the **Coordinator Jobs** tab, you see information similar to the following image:</span></span>

    ![koordinátor feladatok lap](./media/hdinsight-use-oozie-linux-mac/coordinatorjob.png)

    <span data-ttu-id="cfd13-316">A **következő Materialization** -bejegyzése tartalmazza a feladat fut. a következő újraindításkor.</span><span class="sxs-lookup"><span data-stu-id="cfd13-316">The **Next Materialization** entry contains the next time that the job runs.</span></span>

8. <span data-ttu-id="cfd13-317">Hasonló a korábbi munkafolyamat-feladatot, válassza ki a projekt tételt a webes felhasználói felület megjeleníti az információkat a feladat:</span><span class="sxs-lookup"><span data-stu-id="cfd13-317">Similar to the earlier workflow job, selecting the job entry in the web UI displays information on the job:</span></span>

    ![Koordinátor Feladatinformáció](./media/hdinsight-use-oozie-linux-mac/coordinatorjobinfo.png)

    > [!NOTE]
    > <span data-ttu-id="cfd13-319">Ez a rendszerkép csak a feladat sikeres futtatása az ütemezett munkafolyamaton belül nem az egyéni műveletek jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="cfd13-319">This image only shows successful runs of the job, not individual actions within the scheduled workflow.</span></span> <span data-ttu-id="cfd13-320">Amely megtekintéséhez válasszon a **művelet** bejegyzéseket.</span><span class="sxs-lookup"><span data-stu-id="cfd13-320">To see that, select one of the **Action** entries.</span></span>

    ![A művelet adatai](./media/hdinsight-use-oozie-linux-mac/coordinatoractionjob.png)

## <a name="troubleshooting"></a><span data-ttu-id="cfd13-322">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="cfd13-322">Troubleshooting</span></span>

<span data-ttu-id="cfd13-323">A Oozie felhasználói felület lehetővé teszi Oozie naplókat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-323">The Oozie UI allows you to view Oozie logs.</span></span> <span data-ttu-id="cfd13-324">A munkafolyamat által elindított MapReduce-feladatok JobTracker naplókat mutató hivatkozásokat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cfd13-324">It also contains links to JobTracker logs for MapReduce tasks started by the workflow.</span></span> <span data-ttu-id="cfd13-325">A minta hibaelhárítási kell lennie:</span><span class="sxs-lookup"><span data-stu-id="cfd13-325">The pattern for troubleshooting should be:</span></span>

1. <span data-ttu-id="cfd13-326">Tekintse meg a feladatot Oozie webes felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="cfd13-326">View the job in Oozie Web UI.</span></span>

2. <span data-ttu-id="cfd13-327">Ha egy hiba vagy egy adott művelet sikertelen, válassza ki a művelet, ha a **hibaüzenet** mező további információkat tartalmazza a hiba esetén.</span><span class="sxs-lookup"><span data-stu-id="cfd13-327">If there is an error or failure for a specific action, select the action to see if the **Error Message** field provides more information on the failure.</span></span>

3. <span data-ttu-id="cfd13-328">Ha elérhető, a művelet az URL-CÍMÉT segítségével további részleteket (például a Hadoopból naplók) a művelet.</span><span class="sxs-lookup"><span data-stu-id="cfd13-328">If available, use the URL from the action to view more details (such as JobTracker logs) for the action.</span></span>

<span data-ttu-id="cfd13-329">A következő bizonyos hibákat tapasztalhat, és azok megoldását.</span><span class="sxs-lookup"><span data-stu-id="cfd13-329">The following are specific errors you may encounter, and how to resolve them.</span></span>

### <a name="ja009-cannot-initialize-cluster"></a><span data-ttu-id="cfd13-330">JA009: Nem lehet inicializálni a fürt</span><span class="sxs-lookup"><span data-stu-id="cfd13-330">JA009: Cannot initialize cluster</span></span>

<span data-ttu-id="cfd13-331">**A jelenség**: A feladat állapota **FELFÜGGESZTETT**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-331">**Symptoms**: The job status changes to **SUSPENDED**.</span></span> <span data-ttu-id="cfd13-332">A feladat részleteinek megjelenítése a RunHiveScript állapotának **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-332">Details for the job show the RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="cfd13-333">A művelet kiválasztása a következő hibaüzenetet jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="cfd13-333">Selecting the action displays the following error message:</span></span>

    JA009: Cannot initialize Cluster. Please check your configuration for map

<span data-ttu-id="cfd13-334">**OK**: A WASB használt címek a **job.xml** fájl nem tartalmaz, a tároló vagy a tárfiók nevét.</span><span class="sxs-lookup"><span data-stu-id="cfd13-334">**Cause**: The WASB addresses used in the **job.xml** file do not contain the storage container or storage account name.</span></span> <span data-ttu-id="cfd13-335">A WASB cím formátumban kell megadni `wasb://containername@storageaccountname.blob.core.windows.net`.</span><span class="sxs-lookup"><span data-stu-id="cfd13-335">The WASB address format must be `wasb://containername@storageaccountname.blob.core.windows.net`.</span></span>

<span data-ttu-id="cfd13-336">**Megoldási**: módosítsa a WASB címeket, a feladat használja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-336">**Resolution**: Change the WASB addresses used by the job.</span></span>

### <a name="ja002-oozie-is-not-allowed-to-impersonate-ltuser"></a><span data-ttu-id="cfd13-337">JA002: Oozie nem lehet megszemélyesíteni &lt;felhasználó ></span><span class="sxs-lookup"><span data-stu-id="cfd13-337">JA002: Oozie is not allowed to impersonate &lt;USER></span></span>

<span data-ttu-id="cfd13-338">**A jelenség**: A feladat állapota **FELFÜGGESZTETT**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-338">**Symptoms**: The job status changes to **SUSPENDED**.</span></span> <span data-ttu-id="cfd13-339">A feladat részleteinek megjelenítése a RunHiveScript állapotának **START_MANUAL**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-339">Details for the job show the RunHiveScript status as **START_MANUAL**.</span></span> <span data-ttu-id="cfd13-340">A művelet kiválasztása jeleníti meg a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="cfd13-340">Selecting the action shows the following error message:</span></span>

    JA002: User: oozie is not allowed to impersonate <USER>

<span data-ttu-id="cfd13-341">**OK**: jelenlegi engedélybeállítások nem teszik lehetővé az Oozie megszemélyesíthet-e a megadott felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="cfd13-341">**Cause**: Current permission settings do not allow Oozie to impersonate the specified user account.</span></span>

<span data-ttu-id="cfd13-342">**Megoldási**: Oozie megszemélyesíteni a felhasználók számára engedélyezett a **felhasználók** csoport.</span><span class="sxs-lookup"><span data-stu-id="cfd13-342">**Resolution**: Oozie is allowed to impersonate users in the **users** group.</span></span> <span data-ttu-id="cfd13-343">Használja a `groups USERNAME` megtekintéséhez a csoportokat, amelyek a felhasználói fiók tagja.</span><span class="sxs-lookup"><span data-stu-id="cfd13-343">Use the `groups USERNAME` to see the groups that the user account is a member of.</span></span> <span data-ttu-id="cfd13-344">Ha a felhasználó nem tagja a **felhasználók** csoportjában vegye fel a felhasználót a csoportba a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cfd13-344">If the user is not a member of the **users** group, use the following command to add the user to the group:</span></span>

    sudo adduser USERNAME users

> [!NOTE]
> <span data-ttu-id="cfd13-345">HDInsight felismeri, hogy a felhasználó hozzáadta-e a csoport több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="cfd13-345">It may take several minutes before HDInsight recognizes that the user has been added to the group.</span></span>

### <a name="launcher-error-sqoop"></a><span data-ttu-id="cfd13-346">Hiba (Sqoop) indítója</span><span class="sxs-lookup"><span data-stu-id="cfd13-346">Launcher ERROR (Sqoop)</span></span>

<span data-ttu-id="cfd13-347">**A jelenség**: A feladat állapota **KILLED**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-347">**Symptoms**: The job status changes to **KILLED**.</span></span> <span data-ttu-id="cfd13-348">A feladat részleteinek megjelenítése a RunSqoopExport állapotának **hiba**.</span><span class="sxs-lookup"><span data-stu-id="cfd13-348">Details for the job show the RunSqoopExport status as **ERROR**.</span></span> <span data-ttu-id="cfd13-349">A művelet kiválasztása jeleníti meg a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="cfd13-349">Selecting the action shows the following error message:</span></span>

    Launcher ERROR, reason: Main class [org.apache.oozie.action.hadoop.SqoopMain], exit code [1]

<span data-ttu-id="cfd13-350">**OK**: Sqoop nem tudja betölteni az adatbázis eléréséhez szükséges adatbázis-illesztőprogramját.</span><span class="sxs-lookup"><span data-stu-id="cfd13-350">**Cause**: Sqoop is unable to load the database driver required to access the database.</span></span>

<span data-ttu-id="cfd13-351">**Megoldási**: amikor egy Oozie-feladat a Sqoop használ, meg kell adni a feladat által használt adatbázis illesztőprogram más erőforrások (például a workflow.xml).</span><span class="sxs-lookup"><span data-stu-id="cfd13-351">**Resolution**: When using Sqoop from an Oozie job, you must include the database driver with the other resources (such as the workflow.xml) used by the job.</span></span> <span data-ttu-id="cfd13-352">Az adatbázis-illesztőprogramját tartalmazó archív is hivatkozhat a `<sqoop>...</sqoop>` a workflow.xml szakasza.</span><span class="sxs-lookup"><span data-stu-id="cfd13-352">Also Reference the archive containing the database driver from the `<sqoop>...</sqoop>` section of the workflow.xml.</span></span>

<span data-ttu-id="cfd13-353">Például a feladat ebben a dokumentumban, akkor használja az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-353">For example, for the job in this document, you would use the following steps:</span></span>

1. <span data-ttu-id="cfd13-354">Másolja a sqljdbc4.1.jar fájlt a /tutorials/useoozie könyvtár:</span><span class="sxs-lookup"><span data-stu-id="cfd13-354">Copy the sqljdbc4.1.jar file to the /tutorials/useoozie directory:</span></span>

    ```
    hdfs dfs -put /usr/share/java/sqljdbc_4.1/enu/sqljdbc41.jar /tutorials/useoozie/sqljdbc41.jar
    ```

2. <span data-ttu-id="cfd13-355">Módosítsa a következő XML-kód hozzáadása a fenti új sor workflow.xml `</sqoop>`:</span><span class="sxs-lookup"><span data-stu-id="cfd13-355">Modify the workflow.xml to add the following XML on a new line above `</sqoop>`:</span></span>

    ```xml
    <archive>sqljdbc41.jar</archive>
    ```

## <a name="next-steps"></a><span data-ttu-id="cfd13-356">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cfd13-356">Next steps</span></span>

<span data-ttu-id="cfd13-357">Ebben az oktatóanyagban megtanulta, hogyan adhat meg egy Oozie munkafolyamat és az Oozie-feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="cfd13-357">In this tutorial, you learned how to define an Oozie workflow and how to run an Oozie job.</span></span> <span data-ttu-id="cfd13-358">A HDInsight használata kapcsolatos további tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="cfd13-358">To learn more about working with HDInsight, see the following articles:</span></span>

* <span data-ttu-id="cfd13-359">[Időalapú Oozie-koordinátor használata a hdinsight eszközzel][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="cfd13-359">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="cfd13-360">[Hdinsight Hadoop-feladatokat az adatok feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="cfd13-360">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="cfd13-361">[Sqoop használata a hadooppal a Hdinsightban][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="cfd13-361">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="cfd13-362">[A Hive használata a hdinsight Hadoop][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="cfd13-362">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="cfd13-363">[A Pig használata a HDInsight Hadoop][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="cfd13-363">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="cfd13-364">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="cfd13-364">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

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
