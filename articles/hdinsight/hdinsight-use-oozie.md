---
title: hdinsight Hadoop Oozie aaaUse |} Microsoft Docs
description: "Hadoop Oozie használata a Hdinsightban, big data-szolgáltatása. Megtudhatja, hogyan toodefine egy Oozie munkafolyamatot, és az Oozie feladat elküldéséhez."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 870098f0-f416-4491-9719-78994bf4a369
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 12d0cf1a01838ab0f4e699c384ce2fb18f85cbad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-oozie-with-hadoop-toodefine-and-run-a-workflow-in-hdinsight"></a><span data-ttu-id="25fec-104">Hadoop toodefine Oozie használjon, és a munkafolyamat futtatását a Hdinsightban</span><span class="sxs-lookup"><span data-stu-id="25fec-104">Use Oozie with Hadoop toodefine and run a workflow in HDInsight</span></span>
[!INCLUDE [oozie-selector](../../includes/hdinsight-oozie-selector.md)]

<span data-ttu-id="25fec-105">Ismerje meg, hogyan toouse Apache Oozie toodefine munkafolyamat és a Futtatás hello munkafolyamat a hdinsight platformon.</span><span class="sxs-lookup"><span data-stu-id="25fec-105">Learn how toouse Apache Oozie toodefine a workflow and run hello workflow on HDInsight.</span></span> <span data-ttu-id="25fec-106">toolearn hello Oozie coordinator, kapcsolatban lásd: [időalapú Hadoop Oozie-koordinátor használata a HDInsight][hdinsight-oozie-coordinator-time].</span><span class="sxs-lookup"><span data-stu-id="25fec-106">toolearn about hello Oozie coordinator, see [Use time-based Hadoop Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time].</span></span> <span data-ttu-id="25fec-107">Azure Data Factory toolearn lásd [használja a Pig és a Data Factory Hive][azure-data-factory-pig-hive].</span><span class="sxs-lookup"><span data-stu-id="25fec-107">toolearn Azure Data Factory, see [Use Pig and Hive with Data Factory][azure-data-factory-pig-hive].</span></span>

<span data-ttu-id="25fec-108">Apache Oozie egy munkafolyamat/koordinációs rendszer, amely a Hadoop-feladatokat kezeli.</span><span class="sxs-lookup"><span data-stu-id="25fec-108">Apache Oozie is a workflow/coordination system that manages Hadoop jobs.</span></span> <span data-ttu-id="25fec-109">Hadoop-veremmel hello integrálva van, és támogatja a Hadoop-feladatokat Apache MapReduce, Apache Pig, Apache Hive és Apache Sqoop.</span><span class="sxs-lookup"><span data-stu-id="25fec-109">It is integrated with hello Hadoop stack, and it supports Hadoop jobs for Apache MapReduce, Apache Pig, Apache Hive, and Apache Sqoop.</span></span> <span data-ttu-id="25fec-110">Azt is, amelyek adott tooa rendszer, például Java programok vagy héjparancsfájlok ütemezésére használt tooschedule feladatok.</span><span class="sxs-lookup"><span data-stu-id="25fec-110">It can also be used tooschedule jobs that are specific tooa system, like Java programs or shell scripts.</span></span>

<span data-ttu-id="25fec-111">Ebben az oktatóanyagban hello utasításokat követve megvalósítása hello munkafolyamat két műveletet tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="25fec-111">hello workflow you implement by following hello instructions in this tutorial contains two actions:</span></span>

![Munkafolyamat diagramja][img-workflow-diagram]

1. <span data-ttu-id="25fec-113">A Hive művelet a futás a HiveQL parancsfájl toocount hello napló szintű típusonkénti előfordulások log4j fájlban.</span><span class="sxs-lookup"><span data-stu-id="25fec-113">A Hive action runs a HiveQL script toocount hello occurrences of each log-level type in a log4j file.</span></span> <span data-ttu-id="25fec-114">Minden egyes log4j fájl megjelenítő hello típusától és súlyosságától hello, például [NAPLÓZÁSI szint] mező tartalmazó sor mezők áll:</span><span class="sxs-lookup"><span data-stu-id="25fec-114">Each log4j file consists of a line of fields that contains a [LOG LEVEL] field that shows hello type and hello severity, for example:</span></span>
   
        2012-02-03 18:35:34 SampleClass6 [INFO] everything normal for id 577725851
        2012-02-03 18:35:34 SampleClass4 [FATAL] system problem at id 1991281254
        2012-02-03 18:35:34 SampleClass3 [DEBUG] detail for id 1304807656
        ...
   
    <span data-ttu-id="25fec-115">hello Hive parancsfájl kimenetében hasonlít:</span><span class="sxs-lookup"><span data-stu-id="25fec-115">hello Hive script output is similar to:</span></span>
   
        [DEBUG] 434
        [ERROR] 3
        [FATAL] 1
        [INFO]  96
        [TRACE] 816
        [WARN]  4
   
    <span data-ttu-id="25fec-116">További információ a Hive-ról: [A Hive használata a HDInsightban][hdinsight-use-hive].</span><span class="sxs-lookup"><span data-stu-id="25fec-116">For more information about Hive, see [Use Hive with HDInsight][hdinsight-use-hive].</span></span>
2. <span data-ttu-id="25fec-117">A Sqoop művelet hello HiveQL tooa eredménytábla Azure SQL adatbázis exportálja.</span><span class="sxs-lookup"><span data-stu-id="25fec-117">A Sqoop action exports hello HiveQL output tooa table in an Azure SQL database.</span></span> <span data-ttu-id="25fec-118">További információ a Sqoop: [Hadoop Sqoop használata a hdinsightban][hdinsight-use-sqoop].</span><span class="sxs-lookup"><span data-stu-id="25fec-118">For more information about Sqoop, see [Use Hadoop Sqoop with HDInsight][hdinsight-use-sqoop].</span></span>

> [!NOTE]
> <span data-ttu-id="25fec-119">Tekintse meg a HDInsight-fürtökön támogatott Oozie verziók [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?] [hdinsight-versions].</span><span class="sxs-lookup"><span data-stu-id="25fec-119">For supported Oozie versions on HDInsight clusters, see [What's new in hello Hadoop cluster versions provided by HDInsight?][hdinsight-versions].</span></span>
> 
> 

### <a name="prerequisites"></a><span data-ttu-id="25fec-120">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25fec-120">Prerequisites</span></span>
<span data-ttu-id="25fec-121">Ez az oktatóanyag megkezdése előtt rendelkeznie kell hello elem a következő:</span><span class="sxs-lookup"><span data-stu-id="25fec-121">Before you begin this tutorial, you must have hello following item:</span></span>

* <span data-ttu-id="25fec-122">**Munkaállomás Azure PowerShell-lel**.</span><span class="sxs-lookup"><span data-stu-id="25fec-122">**A workstation with Azure PowerShell**.</span></span> 
  

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]
  

## <a name="define-oozie-workflow-and-hello-related-hiveql-script"></a><span data-ttu-id="25fec-123">Adja meg az Oozie munkafolyamat és a hello kapcsolódó HiveQL-parancsfájlt</span><span class="sxs-lookup"><span data-stu-id="25fec-123">Define Oozie workflow and hello related HiveQL script</span></span>
<span data-ttu-id="25fec-124">Oozie munkafolyamatok definíciók hPDL (egy XML folyamat Definition Language) nyelven íródtak.</span><span class="sxs-lookup"><span data-stu-id="25fec-124">Oozie workflows definitions are written in hPDL (a XML Process Definition Language).</span></span> <span data-ttu-id="25fec-125">hello alapértelmezett munkafolyamat Fájlnév *workflow.xml*.</span><span class="sxs-lookup"><span data-stu-id="25fec-125">hello default workflow file name is *workflow.xml*.</span></span> <span data-ttu-id="25fec-126">hello az alábbiakban az hello munkafolyamat fájl ebben az oktatóanyagban használja.</span><span class="sxs-lookup"><span data-stu-id="25fec-126">hello following is hello workflow file you use in this tutorial.</span></span>

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

<span data-ttu-id="25fec-127">Nincsenek definiált hello munkafolyamat két műveleteket.</span><span class="sxs-lookup"><span data-stu-id="25fec-127">There are two actions defined in hello workflow.</span></span> <span data-ttu-id="25fec-128">hello start-tooaction van *RunHiveScript*.</span><span class="sxs-lookup"><span data-stu-id="25fec-128">hello start-tooaction is *RunHiveScript*.</span></span> <span data-ttu-id="25fec-129">Hello művelet sikeresen lefutott, ha van-e a következő művelet hello *RunSqoopExport*.</span><span class="sxs-lookup"><span data-stu-id="25fec-129">If hello action runs successfully, hello next action is *RunSqoopExport*.</span></span>

<span data-ttu-id="25fec-130">hello RunHiveScript több változót tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="25fec-130">hello RunHiveScript has several variables.</span></span> <span data-ttu-id="25fec-131">Ha az Azure PowerShell használatával a munkaállomáson a hello Oozie feladat elküldéséhez. át hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="25fec-131">You pass hello values when you submit hello Oozie job from your workstation by using Azure PowerShell.</span></span>

<table border = "1">
<tr><th><span data-ttu-id="25fec-132">Munkafolyamat-változók</span><span class="sxs-lookup"><span data-stu-id="25fec-132">Workflow variables</span></span></th><th><span data-ttu-id="25fec-133">Leírás</span><span class="sxs-lookup"><span data-stu-id="25fec-133">Description</span></span></th></tr>
<tr><td><span data-ttu-id="25fec-134">${jobTracker}</span><span class="sxs-lookup"><span data-stu-id="25fec-134">${jobTracker}</span></span></td><td><span data-ttu-id="25fec-135">Adja meg a Hadoop-feladat követő hello hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="25fec-135">Specifies hello URL of hello Hadoop job tracker.</span></span> <span data-ttu-id="25fec-136">Használjon <strong>jobtrackerhost:9010</strong> hdinsight 3.0-s és a 2.1-es verzióját.</span><span class="sxs-lookup"><span data-stu-id="25fec-136">Use <strong>jobtrackerhost:9010</strong> in HDInsight version 3.0 and 2.1.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-137">${nameNode}</span><span class="sxs-lookup"><span data-stu-id="25fec-137">${nameNode}</span></span></td><td><span data-ttu-id="25fec-138">Megadja a hello Hadoop neve csomópont hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="25fec-138">Specifies hello URL of hello Hadoop name node.</span></span> <span data-ttu-id="25fec-139">Használjon hello alapértelmezett fájl rendszer címet, például <i>wasb: / /&lt;containerName&gt;@&lt;storageAccountName&gt;. blob.core.windows.net</i>.</span><span class="sxs-lookup"><span data-stu-id="25fec-139">Use hello default file system address, for example, <i>wasb://&lt;containerName&gt;@&lt;storageAccountName&gt;.blob.core.windows.net</i>.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-140">${queueName}</span><span class="sxs-lookup"><span data-stu-id="25fec-140">${queueName}</span></span></td><td><span data-ttu-id="25fec-141">Adja meg a feladat hello hello várólistacímke elküldve.</span><span class="sxs-lookup"><span data-stu-id="25fec-141">Specifies hello queue name that hello job is submitted to.</span></span> <span data-ttu-id="25fec-142">Használjon hello <strong>alapértelmezett</strong>.</span><span class="sxs-lookup"><span data-stu-id="25fec-142">Use hello <strong>default</strong>.</span></span></td></tr>
</table>

<table border = "1">
<tr><th><span data-ttu-id="25fec-143">Műveleti változó struktúra</span><span class="sxs-lookup"><span data-stu-id="25fec-143">Hive action variable</span></span></th><th><span data-ttu-id="25fec-144">Leírás</span><span class="sxs-lookup"><span data-stu-id="25fec-144">Description</span></span></th></tr>
<tr><td><span data-ttu-id="25fec-145">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="25fec-145">${hiveDataFolder}</span></span></td><td><span data-ttu-id="25fec-146">Megadja a hello forráskönyvtár az hello Hive Create Table parancsot.</span><span class="sxs-lookup"><span data-stu-id="25fec-146">Specifies hello source directory for hello Hive Create Table command.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-147">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="25fec-147">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="25fec-148">Hello FELÜLÍRÁSA INSERT utasítás hello kimeneti mappáját adja meg.</span><span class="sxs-lookup"><span data-stu-id="25fec-148">Specifies hello output folder for hello INSERT OVERWRITE statement.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-149">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="25fec-149">${hiveTableName}</span></span></td><td><span data-ttu-id="25fec-150">Hello Hive tábla által hivatkozott hello log4j adatfájlok hello nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="25fec-150">Specifies hello name of hello Hive table that references hello log4j data files.</span></span></td></tr>
</table>

<table border = "1">
<tr><th><span data-ttu-id="25fec-151">Sqoop művelet változó</span><span class="sxs-lookup"><span data-stu-id="25fec-151">Sqoop action variable</span></span></th><th><span data-ttu-id="25fec-152">Leírás</span><span class="sxs-lookup"><span data-stu-id="25fec-152">Description</span></span></th></tr>
<tr><td><span data-ttu-id="25fec-153">${sqlDatabaseConnectionString}</span><span class="sxs-lookup"><span data-stu-id="25fec-153">${sqlDatabaseConnectionString}</span></span></td><td><span data-ttu-id="25fec-154">Megadja a hello Azure SQL adatbázis-kapcsolati karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="25fec-154">Specifies hello Azure SQL database connection string.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-155">${sqlDatabaseTableName}</span><span class="sxs-lookup"><span data-stu-id="25fec-155">${sqlDatabaseTableName}</span></span></td><td><span data-ttu-id="25fec-156">Megadja, ahol hello adatok exportált hello Azure SQL-adatbázistáblában szereplő.</span><span class="sxs-lookup"><span data-stu-id="25fec-156">Specifies hello Azure SQL database table where hello data is exported to.</span></span></td></tr>
<tr><td><span data-ttu-id="25fec-157">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="25fec-157">${hiveOutputFolder}</span></span></td><td><span data-ttu-id="25fec-158">Hello kimeneti mappáját hello utasítás Hive BESZÚRÁSA FELÜLÍRÁSÁHOZ adja meg.</span><span class="sxs-lookup"><span data-stu-id="25fec-158">Specifies hello output folder for hello Hive INSERT OVERWRITE statement.</span></span> <span data-ttu-id="25fec-159">Ez a hello ugyanabban a mappában hello Sqoop export (Exportálás-dir).</span><span class="sxs-lookup"><span data-stu-id="25fec-159">This is hello same folder for hello Sqoop export (export-dir).</span></span></td></tr>
</table>

<span data-ttu-id="25fec-160">Oozie munkafolyamat és a munkafolyamat-műveleteket használatával kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).</span><span class="sxs-lookup"><span data-stu-id="25fec-160">For more information about Oozie workflow and using workflow actions, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight version 2.1).</span></span>

<span data-ttu-id="25fec-161">hello Hive művelet hello munkafolyamat meghívja a HiveQL-parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="25fec-161">hello Hive action in hello workflow calls a HiveQL script file.</span></span> <span data-ttu-id="25fec-162">A parancsfájl három HiveQL utasításokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="25fec-162">This script file contains three HiveQL statements:</span></span>

    DROP TABLE ${hiveTableName};
    CREATE EXTERNAL TABLE ${hiveTableName}(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) ROW FORMAT DELIMITED FIELDS TERMINATED BY ' ' STORED AS TEXTFILE LOCATION '${hiveDataFolder}';
    INSERT OVERWRITE DIRECTORY '${hiveOutputFolder}' SELECT t4 AS sev, COUNT(*) AS cnt FROM ${hiveTableName} WHERE t4 LIKE '[%' GROUP BY t4;

1. <span data-ttu-id="25fec-163">**hello DROP TABLE utasítás** törlések hello log4j Hive táblát, ha van ilyen.</span><span class="sxs-lookup"><span data-stu-id="25fec-163">**hello DROP TABLE statement** deletes hello log4j Hive table if it exists.</span></span>
2. <span data-ttu-id="25fec-164">**CREATE TABLE utasítás hello** log4j Hive külső tábla létrehozása, amely hello log4j naplófájl helye toohello mutat.</span><span class="sxs-lookup"><span data-stu-id="25fec-164">**hello CREATE TABLE statement** creates a log4j Hive external table that points toohello location of hello log4j log file.</span></span> <span data-ttu-id="25fec-165">hello mező határoló ",".</span><span class="sxs-lookup"><span data-stu-id="25fec-165">hello field delimiter is ",".</span></span> <span data-ttu-id="25fec-166">hello alapértelmezett sor elválasztó karaktere "\n".</span><span class="sxs-lookup"><span data-stu-id="25fec-166">hello default line delimiter is "\n".</span></span> <span data-ttu-id="25fec-167">A Hive külső tábla használt tooavoid hello adatfájl távolít el hello eredeti helyre, ha azt szeretné, toorun hello Oozie munkafolyamat több alkalommal.</span><span class="sxs-lookup"><span data-stu-id="25fec-167">A Hive external table is used tooavoid hello data file being removed from hello original location if you want toorun hello Oozie workflow multiple times.</span></span>
3. <span data-ttu-id="25fec-168">**hello FELÜLÍRÁSA INSERT utasítás** hello log4j Hive tábla napló szintű típusonkénti hello előfordulások száma, és menti a hello kimeneti tooa blob az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="25fec-168">**hello INSERT OVERWRITE statement** counts hello occurrences of each log-level type from hello log4j Hive table, and saves hello output tooa blob in Azure Storage.</span></span>

<span data-ttu-id="25fec-169">Három változók hello parancsfájl használatban van:</span><span class="sxs-lookup"><span data-stu-id="25fec-169">There are three variables used in hello script:</span></span>

* <span data-ttu-id="25fec-170">${hiveTableName}</span><span class="sxs-lookup"><span data-stu-id="25fec-170">${hiveTableName}</span></span>
* <span data-ttu-id="25fec-171">${hiveDataFolder}</span><span class="sxs-lookup"><span data-stu-id="25fec-171">${hiveDataFolder}</span></span>
* <span data-ttu-id="25fec-172">${hiveOutputFolder}</span><span class="sxs-lookup"><span data-stu-id="25fec-172">${hiveOutputFolder}</span></span>

<span data-ttu-id="25fec-173">hello munkafolyamat csomagdefiníciós fájl (ebben az oktatóanyagban workflow.xml) HiveQL-parancsfájlt továbbítja ezeket az értékeket toothis futási időben.</span><span class="sxs-lookup"><span data-stu-id="25fec-173">hello workflow definition file (workflow.xml in this tutorial) passes these values toothis HiveQL script at run time.</span></span>

<span data-ttu-id="25fec-174">A blob-tároló hello munkafolyamat fájl- és a HiveQL fájl hello tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="25fec-174">Both hello workflow file and hello HiveQL file are stored in a blob container.</span></span>  <span data-ttu-id="25fec-175">PowerShell-parancsprogram, az oktatóanyag későbbi részében hello mindkét fájlok toohello alapértelmezett tárfiók másolja.</span><span class="sxs-lookup"><span data-stu-id="25fec-175">hello PowerShell script you use later in this tutorial copies both files toohello default Storage account.</span></span> 

## <a name="submit-oozie-jobs-using-powershell"></a><span data-ttu-id="25fec-176">PowerShell-lel Oozie feladatok elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="25fec-176">Submit Oozie jobs using PowerShell</span></span>
<span data-ttu-id="25fec-177">Az Azure PowerShell jelenleg nem biztosít semmilyen parancsmagok Oozie feladatok meghatározásához.</span><span class="sxs-lookup"><span data-stu-id="25fec-177">Azure PowerShell currently doesn't provide any cmdlets for defining Oozie jobs.</span></span> <span data-ttu-id="25fec-178">Használhatja a hello **Invoke-RestMethod** parancsmag tooinvoke Oozie webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="25fec-178">You can use hello **Invoke-RestMethod** cmdlet tooinvoke Oozie web services.</span></span> <span data-ttu-id="25fec-179">hello Oozie webszolgáltatások API egy HTTP REST API-t JSON.</span><span class="sxs-lookup"><span data-stu-id="25fec-179">hello Oozie web services API is a HTTP REST JSON API.</span></span> <span data-ttu-id="25fec-180">Hello Oozie webszolgáltatások API kapcsolatos további információkért lásd: [Apache Oozie 4.0 dokumentáció] [ apache-oozie-400] (a HDInsight 3.0-s verziója) vagy [Apache Oozie 3.3.2 dokumentáció] [ apache-oozie-332] (a HDInsight 2.1-es verzió).</span><span class="sxs-lookup"><span data-stu-id="25fec-180">For more information about hello Oozie web services API, see [Apache Oozie 4.0 documentation][apache-oozie-400] (for HDInsight version 3.0) or [Apache Oozie 3.3.2 documentation][apache-oozie-332] (for HDInsight version 2.1).</span></span>

<span data-ttu-id="25fec-181">Ebben a szakaszban a PowerShell parancsfájl hello hello a lépéseket követve hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="25fec-181">hello PowerShell script in this section performs hello following steps:</span></span>

1. <span data-ttu-id="25fec-182">Csatlakozás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="25fec-182">Connect tooAzure.</span></span>
2. <span data-ttu-id="25fec-183">Azure-erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="25fec-183">Create an Azure resource group.</span></span> <span data-ttu-id="25fec-184">További információkért lásd: [használata Azure PowerShell használata Azure Resource Managerrel](../powershell-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="25fec-184">For more information, see [Use Azure PowerShell with Azure Resource Manager](../powershell-azure-resource-manager.md).</span></span>
3. <span data-ttu-id="25fec-185">Hozzon létre egy Azure SQL adatbázis-kiszolgáló, az Azure SQL-adatbázis és a két tábla.</span><span class="sxs-lookup"><span data-stu-id="25fec-185">Create an Azure SQL Database server, an Azure SQL database, and two tables.</span></span> <span data-ttu-id="25fec-186">Ezek hello Sqoop művelet hello munkafolyamat által használt.</span><span class="sxs-lookup"><span data-stu-id="25fec-186">These are used by hello Sqoop action in hello workflow.</span></span>
   
    <span data-ttu-id="25fec-187">hello táblázat neve *log4jLogCount*.</span><span class="sxs-lookup"><span data-stu-id="25fec-187">hello table name is *log4jLogCount*.</span></span>
4. <span data-ttu-id="25fec-188">Hozzon létre egy HDInsight-fürt használt toorun Oozie feladatokat.</span><span class="sxs-lookup"><span data-stu-id="25fec-188">Create an HDInsight cluster used toorun Oozie jobs.</span></span>
   
    <span data-ttu-id="25fec-189">tooexamine hello fürt, használhatja hello Azure-portálon vagy az Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="25fec-189">tooexamine hello cluster, you can use hello Azure portal or Azure PowerShell.</span></span>
5. <span data-ttu-id="25fec-190">Másolja a hello oozie munkafolyamat fájl- és hello HiveQL parancsfájl fájl toohello alapértelmezett fájlrendszert.</span><span class="sxs-lookup"><span data-stu-id="25fec-190">Copy hello oozie workflow file and hello HiveQL script file toohello default file system.</span></span>
   
    <span data-ttu-id="25fec-191">Mindkét fájljai a következő nyilvános blobtárolóban.</span><span class="sxs-lookup"><span data-stu-id="25fec-191">Both files are stored in a public Blob container.</span></span>
   
   * <span data-ttu-id="25fec-192">Másolja a hello HiveQL parancsfájl (useoozie.hql) tooAzure Storage (wasb:///tutorials/useoozie/useoozie.hql).</span><span class="sxs-lookup"><span data-stu-id="25fec-192">Copy hello HiveQL script (useoozie.hql) tooAzure Storage (wasb:///tutorials/useoozie/useoozie.hql).</span></span>
   * <span data-ttu-id="25fec-193">Workflow.xml toowasb:///tutorials/useoozie/workflow.xml másolja.</span><span class="sxs-lookup"><span data-stu-id="25fec-193">Copy workflow.xml toowasb:///tutorials/useoozie/workflow.xml.</span></span>
   * <span data-ttu-id="25fec-194">Hello adatok fájl másolása (/ example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span><span class="sxs-lookup"><span data-stu-id="25fec-194">Copy hello data file (/example/data/sample.log) toowasb:///tutorials/useoozie/data/sample.log.</span></span>
6. <span data-ttu-id="25fec-195">Az Oozie feladat küldése</span><span class="sxs-lookup"><span data-stu-id="25fec-195">Submit an Oozie job.</span></span>
   
    <span data-ttu-id="25fec-196">tooexamine hello OOzie feladat eredményeinek, használja a Visual Studio vagy más eszközök tooconnect toohello Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="25fec-196">tooexamine hello OOzie job results, use Visual Studio or other tools tooconnect toohello Azure SQL Database.</span></span>

<span data-ttu-id="25fec-197">Itt található hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="25fec-197">Here is hello script.</span></span>  <span data-ttu-id="25fec-198">A Windows PowerShell ISE hello parancsfájlt is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="25fec-198">You can run hello script from Windows PowerShell ISE.</span></span> <span data-ttu-id="25fec-199">Csak akkor kell tooconfigure hello első 7 változók.</span><span class="sxs-lookup"><span data-stu-id="25fec-199">You only need tooconfigure hello first 7 variables.</span></span>

    #region - provide hello following values

    $subscriptionID = "<Enter your Azure subscription ID>"

    # SQL Database server login credentials used for creating and connecting
    $sqlDatabaseLogin = "<Enter SQL Database Login Name>"
    $sqlDatabasePassword = "<Enter SQL Database Login Password>"

    # HDInsight cluster HTTP user credential used for creating and connectin
    $httpUserName = "admin"  # hello default name is "admin"
    $httpPassword = "<Enter HDInsight Cluster HTTP User Password>"

    # Used for creating Azure service names
    $nameToken = "<Enter an Alias>"
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    #endregion

    #region - variables

    # Resource group variables
    $resourceGroupName = $namePrefix + "rg"
    $location = "East US 2" # used by all Azure services defined in this tutorial

    # SQL database varialbes
    $sqlDatabaseServerName = $namePrefix + "sqldbserver"
    $sqlDatabaseName = $namePrefix + "sqldb"
    $sqlDatabaseConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $sqlDatabaseMaxSizeGB = 10

    # Used for retrieving external IP address and creating firewall rules
    $ipAddressRestService = "http://bot.whatismyipaddress.com"
    $fireWallRuleName = "UseSqoop"

    # HDInsight variables
    $hdinsightClusterName = $namePrefix + "hdi"
    $defaultStorageAccountName = $namePrefix + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    #endregion

    # Treat all errors as terminating
    $ErrorActionPreference = "Stop"

    #region - Connect tooAzure subscription
    Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
    try{Get-AzureRmContext}
    catch{
        Login-AzureRmAccount
        Select-AzureRmSubscription -SubscriptionId $subscriptionID
    }
    #endregion

    #region - Create Azure resouce group
    Write-Host "`nCreating an Azure resource group ..." -ForegroundColor Green
    try{
        Get-AzureRmResourceGroup -Name $resourceGroupName
    }
    catch{
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    }
    #endregion

    #region - Create Azure SQL database server
    Write-Host "`nCreating an Azure SQL Database server ..." -ForegroundColor Green
    try{
        Get-AzureRmSqlServer -ServerName $sqlDatabaseServerName -ResourceGroupName $resourceGroupName}
    catch{
        Write-Host "`nCreating SQL Database server ..."  -ForegroundColor Green

        $sqlDatabasePW = ConvertTo-SecureString -String $sqlDatabasePassword -AsPlainText -Force
        $sqlLoginCredentials = New-Object System.Management.Automation.PSCredential($sqlDatabaseLogin,$sqlDatabasePW)

        $sqlDatabaseServerName = (New-AzureRmSqlServer `
                                    -ResourceGroupName $resourceGroupName `
                                    -ServerName $sqlDatabaseServerName `
                                    -SqlAdministratorCredentials $sqlLoginCredentials `
                                    -Location $location).ServerName
        Write-Host "`tThe new SQL database server name is $sqlDatabaseServerName." -ForegroundColor Cyan

        Write-Host "`nCreating firewall rule, $fireWallRuleName ..." -ForegroundColor Green
        $workstationIPAddress = Invoke-RestMethod $ipAddressRestService
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-workstation" `
            -StartIpAddress $workstationIPAddress `
            -EndIpAddress $workstationIPAddress

        #tooallow other Azure services tooaccess hello server add a firewall rule and set both hello StartIpAddress and EndIpAddress too0.0.0.0. 
        #Note that this allows Azure traffic from any Azure subscription tooaccess hello server.
        New-AzureRmSqlServerFirewallRule `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -FirewallRuleName "$fireWallRuleName-Azureservices" `
            -StartIpAddress "0.0.0.0" `
            -EndIpAddress "0.0.0.0"
    }
    #endregion

    #region - Create and validate Azure SQL database
    Write-Host "`nCreating SQL Database, $sqlDatabaseName ..."  -ForegroundColor Green

    try {
        Get-AzureRmSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName
    }
    catch {
        New-AzureRMSqlDatabase `
            -ResourceGroupName $resourceGroupName `
            -ServerName $sqlDatabaseServerName `
            -DatabaseName $sqlDatabaseName `
            -Edition "Standard" `
            -RequestedServiceObjectiveName "S1"
    }
    #endregion

    #region - Create SQL database tables
    Write-Host "Creating hello log4jlogs table  ..." -ForegroundColor Green

    $sqlDatabaseTableName = "log4jLogsCount"
    $cmdCreateLog4jCountTable = " CREATE TABLE [dbo].[$sqlDatabaseTableName](
            [Level] [nvarchar](10) NOT NULL,
            [Total] float,
        CONSTRAINT [PK_$sqlDatabaseTableName] PRIMARY KEY CLUSTERED
        (
        [Level] ASC
        )
        )"

    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = $sqlDatabaseConnectionString
    $conn.Open()

    # Create hello log4jlogs table and index
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.Connection = $conn
    $cmd.CommandText = $cmdCreateLog4jCountTable
    $cmd.ExecuteNonQuery()

    $conn.close()
    #endregion

    #region - Create HDInsight cluster

    Write-Host "Creating hello HDInsight cluster and hello dependent services ..." -ForegroundColor Green

    # Create hello default storage account
    New-AzureRmStorageAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $defaultStorageAccountName `
        -Location $location `
        -Type Standard_LRS

    # Create hello default Blob container
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                    -ResourceGroupName $resourceGroupName `
                                    -Name $defaultStorageAccountName)[0].Value
    $defaultStorageAccountContext = New-AzureStorageContext `
                                        -StorageAccountName $defaultStorageAccountName `
                                        -StorageAccountKey $defaultStorageAccountKey 
    New-AzureStorageContainer `
        -Name $defaultBlobContainerName `
        -Context $defaultStorageAccountContext 

    # Create hello HDInsight cluster
    $pw = ConvertTo-SecureString -String $httpPassword -AsPlainText -Force
    $httpCredential = New-Object System.Management.Automation.PSCredential($httpUserName,$pw)

    New-AzureRmHDInsightCluster `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $HDInsightClusterName `
        -Location $location `
        -ClusterType Hadoop `
        -OSType Windows `
        -ClusterSizeInNodes 2 `
        -HttpCredential $httpCredential `
        -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultStorageContainer $defaultBlobContainerName 

    # Validate hello cluster
    Get-AzureRmHDInsightCluster -ClusterName $hdinsightClusterName
    #endregion

    #region - copy Oozie workflow and HiveQL files

    Write-Host "Copy workflow definition and HiveQL script file ..." -ForegroundColor Green

    # Both files are stored in a public Blob
    $publicBlobContext = New-AzureStorageContext -StorageAccountName "hditutorialdata" -Anonymous

    # WASB folder for storing hello Oozie tutorial files.
    $destFolder = "tutorials/useoozie"  # Do NOT use hello long path here

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "useooziewf.hql"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/useooziewf.hql" `
        -Force

    Start-CopyAzureStorageBlob `
        -Context $publicBlobContext `
        -SrcContainer "useoozie" `
        -SrcBlob "workflow.xml"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -DestBlob "$destFolder/workflow.xml" `
        -Force

    #validate hello copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/workflow.xml

    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/useooziewf.hql

    #endregion

    #region - copy hello sample.log file

    Write-Host "Make a copy of hello sample.log file ... " -ForegroundColor Green

    Start-CopyAzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -SrcContainer $defaultBlobContainerName `
        -SrcBlob "example/data/sample.log"  `
        -DestContext $defaultStorageAccountContext `
        -DestContainer $defaultBlobContainerName `
        -destBlob "$destFolder/data/sample.log" 

    #validate hello copy
    Get-AzureStorageBlob `
        -Context $defaultStorageAccountContext `
        -Container $defaultBlobContainerName `
        -Blob $destFolder/data/sample.log

    #endregion

    #region - submit Oozie job

    $storageUri="wasb://$defaultBlobContainerName@$defaultStorageAccountName.blob.core.windows.net"

    $oozieJobName = $namePrefix + "OozieJob"

    #Oozie WF variables
    $oozieWFPath="$storageUri/tutorials/useoozie"  # hello default name is workflow.xml. And you don't need toospecify hello file name.
    $waitTimeBetweenOozieJobStatusCheck=10

    #Hive action variables
    $hiveScript = "$storageUri/tutorials/useoozie/useooziewf.hql"
    $hiveTableName = "log4jlogs"
    $hiveDataFolder = "$storageUri/tutorials/useoozie/data"
    $hiveOutputFolder = "$storageUri/tutorials/useoozie/output"

    #Sqoop action variables
    $sqlDatabaseConnectionString = "jdbc:sqlserver://$sqlDatabaseServerName.database.windows.net;user=$sqlDatabaseLogin@$sqlDatabaseServerName;password=$sqlDatabasePassword;database=$sqlDatabaseName"

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
        <value>$httpUserName</value>
    </property>

    <property>
        <name>oozie.wf.application.path</name>
        <value>$oozieWFPath</value>
    </property>

    </configuration>
    "@

    Write-Host "Checking Oozie server status..." -ForegroundColor Green
    $clusterUriStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/admin/status"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriStatus -Credential $httpCredential -OutVariable $OozieServerStatus

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieServerSatus = $jsonResponse[0].("systemMode")
    Write-Host "Oozie server status is $oozieServerSatus."

    # create Oozie job
    Write-Host "Sending hello following Payload toohello cluster:" -ForegroundColor Green
    Write-Host "`n--------`n$OoziePayload`n--------"
    $clusterUriCreateJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/jobs"
    $response = Invoke-RestMethod -Method Post -Uri $clusterUriCreateJob -Credential $httpCredential -Body $OoziePayload -ContentType "application/xml" -OutVariable $OozieJobName #-debug

    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $oozieJobId = $jsonResponse[0].("id")
    Write-Host "Oozie job id is $oozieJobId..."

    # start Oozie job
    Write-Host "Starting hello Oozie job $oozieJobId..." -ForegroundColor Green
    $clusterUriStartJob = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?action=start"
    $response = Invoke-RestMethod -Method Put -Uri $clusterUriStartJob -Credential $httpCredential | Format-Table -HideTableHeaders #-debug

    # get job status
    Write-Host "Sleeping for $waitTimeBetweenOozieJobStatusCheck seconds until hello job metadata is populated in hello Oozie metastore..." -ForegroundColor Green
    Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck

    Write-Host "Getting job status and waiting for hello job toocomplete..." -ForegroundColor Green
    $clusterUriGetJobStatus = "https://$hdinsightClusterName.azurehdinsight.net:443/oozie/v2/job/" + $oozieJobId + "?show=info"
    $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
    $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
    $JobStatus = $jsonResponse[0].("status")

    while($JobStatus -notmatch "SUCCEEDED|KILLED")
    {
        Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state...waiting $waitTimeBetweenOozieJobStatusCheck seconds for hello job toocomplete..."
        Start-Sleep -Seconds $waitTimeBetweenOozieJobStatusCheck
        $response = Invoke-RestMethod -Method Get -Uri $clusterUriGetJobStatus -Credential $httpCredential
        $jsonResponse = ConvertFrom-Json (ConvertTo-Json -InputObject $response)
        $JobStatus = $jsonResponse[0].("status")
        $jobStatus
    }

    Write-Host "$(Get-Date -format 'G'): $oozieJobId is in $JobStatus state!" -ForegroundColor Green

    #endregion


<span data-ttu-id="25fec-200">**futtatási toore hello oktatóanyag**</span><span class="sxs-lookup"><span data-stu-id="25fec-200">**toore-run hello tutorial**</span></span>

<span data-ttu-id="25fec-201">a munkafolyamat futtatási toore hello, törölnie kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="25fec-201">toore-run hello workflow, you must delete hello following items:</span></span>

* <span data-ttu-id="25fec-202">hello Hive parancsfájl kimeneti fájlja</span><span class="sxs-lookup"><span data-stu-id="25fec-202">hello Hive script output file</span></span>
* <span data-ttu-id="25fec-203">hello adatok hello log4jLogsCount táblázatban</span><span class="sxs-lookup"><span data-stu-id="25fec-203">hello data in hello log4jLogsCount table</span></span>

<span data-ttu-id="25fec-204">Íme egy PowerShell-parancsfájlpélda közül választhat:</span><span class="sxs-lookup"><span data-stu-id="25fec-204">Here is a sample PowerShell script that you can use:</span></span>

    $resourceGroupName = "<AzureResourceGroupName>"

    $defaultStorageAccountName = "<AzureStorageAccountName>"
    $defaultBlobContainerName = "<ContainerName>"

    #SQL database variables
    $sqlDatabaseServerName = "<SQLDatabaseServerName>"
    $sqlDatabaseLogin = "<SQLDatabaseLoginName>"
    $sqlDatabasePassword = "<SQLDatabaseLoginPassword>"
    $sqlDatabaseName = "<SQLDatabaseName>"
    $sqlDatabaseTableName = "log4jLogsCount"

    Write-host "Delete hello Hive script output file ..." -ForegroundColor Green
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey `
                                -ResourceGroupName $resourceGroupName `
                                -Name $defaultStorageAccountName)[0].Value
    $destContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $defaultStorageAccountKey
    Remove-AzureStorageBlob -Context $destContext -Blob "tutorials/useoozie/output/000000_0" -Container $defaultBlobContainerName

    Write-host "Delete all hello records from hello log4jLogsCount table ..." -ForegroundColor Green
    $conn = New-Object System.Data.SqlClient.SqlConnection
    $conn.ConnectionString = "Data Source=$sqlDatabaseServerName.database.windows.net;Initial Catalog=$sqlDatabaseName;User ID=$sqlDatabaseLogin;Password=$sqlDatabasePassword;Encrypt=true;Trusted_Connection=false;"
    $conn.open()
    $cmd = New-Object System.Data.SqlClient.SqlCommand
    $cmd.connection = $conn
    $cmd.commandtext = "delete from $sqlDatabaseTableName"
    $cmd.executenonquery()

    $conn.close()

## <a name="next-steps"></a><span data-ttu-id="25fec-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25fec-205">Next steps</span></span>
<span data-ttu-id="25fec-206">Ebben az oktatóanyagban, megtudta, hogyan toodefine egy Oozie munkafolyamat, és hogyan toorun egy Oozie feladat PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="25fec-206">In this tutorial, you learned how toodefine an Oozie workflow and how toorun an Oozie job by using PowerShell.</span></span> <span data-ttu-id="25fec-207">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="25fec-207">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="25fec-208">[Időalapú Oozie-koordinátor használata a hdinsight eszközzel][hdinsight-oozie-coordinator-time]</span><span class="sxs-lookup"><span data-stu-id="25fec-208">[Use time-based Oozie Coordinator with HDInsight][hdinsight-oozie-coordinator-time]</span></span>
* <span data-ttu-id="25fec-209">[Hadoop használatának megkezdésében a Hive HDInsight tooanalyze mobil kézibeszélő használatban][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="25fec-209">[Get started using Hadoop with Hive in HDInsight tooanalyze mobile handset use][hdinsight-get-started]</span></span>
* <span data-ttu-id="25fec-210">[Használhat Azure Blob Storage tárolót a hdinsight eszközzel][hdinsight-storage]</span><span class="sxs-lookup"><span data-stu-id="25fec-210">[Use Azure Blob storage with HDInsight][hdinsight-storage]</span></span>
* <span data-ttu-id="25fec-211">[HDInsight a PowerShell felügyelete][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="25fec-211">[Administer HDInsight using PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="25fec-212">[Hdinsight Hadoop-feladatokat az adatok feltöltése][hdinsight-upload-data]</span><span class="sxs-lookup"><span data-stu-id="25fec-212">[Upload data for Hadoop jobs in HDInsight][hdinsight-upload-data]</span></span>
* <span data-ttu-id="25fec-213">[Sqoop használata a hadooppal a Hdinsightban][hdinsight-use-sqoop]</span><span class="sxs-lookup"><span data-stu-id="25fec-213">[Use Sqoop with Hadoop in HDInsight][hdinsight-use-sqoop]</span></span>
* <span data-ttu-id="25fec-214">[A Hive használata a hdinsight Hadoop][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="25fec-214">[Use Hive with Hadoop on HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="25fec-215">[A Pig használata a HDInsight Hadoop][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="25fec-215">[Use Pig with Hadoop on HDInsight][hdinsight-use-pig]</span></span>
* <span data-ttu-id="25fec-216">[Java-MapReduce programok fejlesztése a HDInsight][hdinsight-develop-mapreduce]</span><span class="sxs-lookup"><span data-stu-id="25fec-216">[Develop Java MapReduce programs for HDInsight][hdinsight-develop-mapreduce]</span></span>

[hdinsight-cmdlets-download]: http://go.microsoft.com/fwlink/?LinkID=325563



[azure-data-factory-pig-hive]: ../data-factory/data-factory-data-transformation-activities.md
[hdinsight-oozie-coordinator-time]: hdinsight-use-oozie-coordinator-time.md
[hdinsight-versions]:  hdinsight-component-versioning.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md


[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[hdinsight-develop-mapreduce]: hdinsight-develop-deploy-java-mapreduce-linux.md

[sqldatabase-create-configue]: ../sql-database-create-configure.md
[sqldatabase-get-started]: ../sql-database-get-started.md

[azure-management-portal]: https://portal.azure.com/
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
