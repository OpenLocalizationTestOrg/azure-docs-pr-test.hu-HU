---
title: "aaaAzure HDInsight hibaelhárítási útmutatók |} Microsoft Docs"
description: "Végezzen hibaelhárítást a Hadoop-munkaterhelések Azure HDInsight használatával. A dokumentáció részletesen bemutatja, hogyan toouse HDInsight toosolve gyakori problémák struktúra, a Spark, a YARN, a HBase, a HDFS és a Storm."
services: hdinsight
author: arijitt
manager: arijitt
layout: LandingPage
ms.assetid: 
ms.service: hdinsight
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: landing - page
ms.date: 7/06/2017
ms.author: arijitt
ms.openlocfilehash: 6d801389cba738345b36364302c62008b8318018
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-by-using-azure-hdinsight"></a><span data-ttu-id="a3be4-104">Hibaelhárítás Azure HDInsight segítségével</span><span class="sxs-lookup"><span data-stu-id="a3be4-104">Troubleshoot by using Azure HDInsight</span></span>

| <span data-ttu-id="a3be4-105">Apache munkaterhelés</span><span class="sxs-lookup"><span data-stu-id="a3be4-105">Apache workload</span></span> | <span data-ttu-id="a3be4-106">Felső kérdések</span><span class="sxs-lookup"><span data-stu-id="a3be4-106">Top questions</span></span> |
|---|---|
|<span data-ttu-id="a3be4-107">![HBase](./media/hdinsight-troubleshoot-guide/HBASE.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-107">![HBase](./media/hdinsight-troubleshoot-guide/HBASE.png)</span></span><br>[<span data-ttu-id="a3be4-108">A HBase hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-108">Troubleshoot HBase</span></span>](hdinsight-troubleshoot-hbase.md)|<br>[<span data-ttu-id="a3be4-109">Hogyan futtathatok hbck parancs jelentéseket a ki nem osztott több régióba</span><span class="sxs-lookup"><span data-stu-id="a3be4-109">How do I run hbck command reports with multiple unassigned regions</span></span>](hdinsight-troubleshoot-hbase.md#how-do-i-run-hbck-command-reports-with-multiple-unassigned-regions)<br><br>[<span data-ttu-id="a3be4-110">Hogyan oldja meg időtúllépés problémák, régió hozzárendelések hbck parancsok használata esetén</span><span class="sxs-lookup"><span data-stu-id="a3be4-110">How do I fix timeout issues when using hbck commands for region assignments</span></span>](hdinsight-troubleshoot-hbase.md#how-do-i-fix-timeout-issues-with-hbck-commands-for-region-assignments)<br><br>[<span data-ttu-id="a3be4-111">Hogyan tegye I kényszerített – letiltása HDFS csökkentett módban egy fürtön</span><span class="sxs-lookup"><span data-stu-id="a3be4-111">How do I force-disable HDFS safe mode on a cluster</span></span>](hdinsight-troubleshoot-hbase.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)<br><br>[<span data-ttu-id="a3be4-112">Hogyan állítsa helyre JDBC vagy SQLLine elérhetőségét Apache Phoenix problémái</span><span class="sxs-lookup"><span data-stu-id="a3be4-112">How do I fix JDBC or SQLLine connectivity issues with Apache Phoenix</span></span>](hdinsight-troubleshoot-hbase.md#how-do-i-fix-jdbc-or-sqlline-connectivity-issues-with-apache-phoenix)<br><br>[<span data-ttu-id="a3be4-113">Mi okozza a főkiszolgálóval toofail toostart</span><span class="sxs-lookup"><span data-stu-id="a3be4-113">What causes a master server toofail toostart</span></span>](hdinsight-troubleshoot-hbase.md#what-causes-a-master-server-to-fail-to-start)<br><br>[<span data-ttu-id="a3be4-114">Mi újraindítás hibát okoz a terület-kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="a3be4-114">What causes a restart failure on a region server</span></span>](hdinsight-troubleshoot-hbase.md#what-causes-a-restart-failure-on-a-region-server)|
|<span data-ttu-id="a3be4-115">![HDFS](./media/hdinsight-troubleshoot-guide/HDFS.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-115">![HDFS](./media/hdinsight-troubleshoot-guide/HDFS.png)</span></span><br>[<span data-ttu-id="a3be4-116">HDFS hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-116">Troubleshoot HDFS</span></span>](hdinsight-troubleshoot-hdfs.md)|<br>[<span data-ttu-id="a3be4-117">Hogyan érhető el a fürtben található a helyi HDFS</span><span class="sxs-lookup"><span data-stu-id="a3be4-117">How do I access a local HDFS from inside a cluster</span></span>](hdinsight-troubleshoot-hdfs.md#how-do-i-access-local-hdfs-from-inside-a-cluster)<br><br>[<span data-ttu-id="a3be4-118">Hogyan tegye I kényszerített – letiltása HDFS csökkentett módban egy fürtön</span><span class="sxs-lookup"><span data-stu-id="a3be4-118">How do I force-disable HDFS safe mode on a cluster</span></span>](hdinsight-troubleshoot-hdfs.md#how-do-i-force-disable-hdfs-safe-mode-in-a-cluster)|
|<span data-ttu-id="a3be4-119">![Hive](./media/hdinsight-troubleshoot-guide/HIVE.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-119">![Hive](./media/hdinsight-troubleshoot-guide/HIVE.png)</span></span><br>[<span data-ttu-id="a3be4-120">A HBase hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-120">Troubleshoot HBase</span></span>](hdinsight-troubleshoot-hive.md)|<br>[<span data-ttu-id="a3be4-121">Hogyan egy Hive metaadattárhoz exportálhatja és importálhatja azokat a fürt egy másik</span><span class="sxs-lookup"><span data-stu-id="a3be4-121">How do I export a Hive metastore and import it on another cluster</span></span>](hdinsight-troubleshoot-hive.md#how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster)<br><br>[<span data-ttu-id="a3be4-122">Hogyan keresik meg Hive naplók fürt</span><span class="sxs-lookup"><span data-stu-id="a3be4-122">How do I locate Hive logs on a cluster</span></span>](hdinsight-troubleshoot-hive.md#how-do-i-locate-hive-logs-on-a-cluster)<br><br>[<span data-ttu-id="a3be4-123">Hogyan indítsa el a Hive rendszerhéjat egy fürtön adott konfigurációjú hello</span><span class="sxs-lookup"><span data-stu-id="a3be4-123">How do I launch hello Hive shell with specific configurations on a cluster</span></span>](hdinsight-troubleshoot-hive.md#how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster)<br><br>[<span data-ttu-id="a3be4-124">Hogyan elemezheti a fürt kritikus útvonalra Tez DAG adatokat</span><span class="sxs-lookup"><span data-stu-id="a3be4-124">How do I analyze Tez DAG data on a cluster-critical path</span></span>](hdinsight-troubleshoot-hive.md#how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path)<br><br>[<span data-ttu-id="a3be4-125">Hogyan le Tez DAG adatok fürtből</span><span class="sxs-lookup"><span data-stu-id="a3be4-125">How do I download Tez DAG data from a cluster</span></span>](hdinsight-troubleshoot-hive.md#how-do-i-download-tez-dag-data-from-a-cluster)|
|<span data-ttu-id="a3be4-126">![Spark](./media/hdinsight-troubleshoot-guide/SPARK.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-126">![Spark](./media/hdinsight-troubleshoot-guide/SPARK.png)</span></span><br>[<span data-ttu-id="a3be4-127">Spark hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-127">Troubleshoot Spark</span></span>](hdinsight-troubleshoot-SPARK.md)|<br>[<span data-ttu-id="a3be4-128">Hogyan konfigurálhatom a Spark-alkalmazások fürtök az Ambari használatával</span><span class="sxs-lookup"><span data-stu-id="a3be4-128">How do I configure a Spark application by using Ambari on clusters</span></span>](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-ambari-on-clusters)<br><br>[<span data-ttu-id="a3be4-129">Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Jupyter notebook használatával</span><span class="sxs-lookup"><span data-stu-id="a3be4-129">How do I configure a Spark application by using a Jupyter notebook on clusters</span></span>](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-a-jupyter-notebook-on-clusters)<br><br>[<span data-ttu-id="a3be4-130">Hogyan konfigurálhatom a Spark-alkalmazások fürtökön Livy használatával</span><span class="sxs-lookup"><span data-stu-id="a3be4-130">How do I configure a Spark application by using Livy on clusters</span></span>](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-livy-on-clusters)<br><br>[<span data-ttu-id="a3be4-131">Hogyan konfigurálhatók a alkalmazás használatával spark-elküldeni egy Spark-fürtökön</span><span class="sxs-lookup"><span data-stu-id="a3be4-131">How do I configure a Spark application by using spark-submit on clusters</span></span>](hdinsight-troubleshoot-spark.md#how-do-i-configure-a-spark-application-by-using-spark-submit-on-clusters)<br><br>[<span data-ttu-id="a3be4-132">Mi okozza a Spark OutOfMemoryError Alkalmazáskivétel</span><span class="sxs-lookup"><span data-stu-id="a3be4-132">What causes a Spark application OutOfMemoryError exception</span></span>](hdinsight-troubleshoot-spark.md#what-causes-a-spark-application-outofmemoryerror-exception)|
|<span data-ttu-id="a3be4-133">![A Storm](./media/hdinsight-troubleshoot-guide/STORM.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-133">![Storm](./media/hdinsight-troubleshoot-guide/STORM.png)</span></span><br>[<span data-ttu-id="a3be4-134">A Storm hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-134">Troubleshoot Storm</span></span>](hdinsight-troubleshoot-STORM.md)|<br>[<span data-ttu-id="a3be4-135">Hogyan érhetem el az egy fürtön található Storm felhasználói felülete hello</span><span class="sxs-lookup"><span data-stu-id="a3be4-135">How do I access hello Storm UI on a cluster</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-access-the-storm-ui-on-a-cluster)<br><br>[<span data-ttu-id="a3be4-136">Hogyan Storm esemény hub spout ellenőrzőpont információinak átvitele egy topológia tooanother</span><span class="sxs-lookup"><span data-stu-id="a3be4-136">How do I transfer Storm event hub spout checkpoint information from one topology tooanother</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another)<br><br>[<span data-ttu-id="a3be4-137">Hogyan keresse meg a Storm bináris fürt</span><span class="sxs-lookup"><span data-stu-id="a3be4-137">How do I locate Storm binaries on a cluster</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-binaries-on-a-cluster)<br><br>[<span data-ttu-id="a3be4-138">Hogyan állapítható meg hello telepítési topológia a Storm-fürt</span><span class="sxs-lookup"><span data-stu-id="a3be4-138">How do I determine hello deployment topology of a Storm cluster</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-determine-the-deployment-topology-of-a-storm-cluster)<br><br>[<span data-ttu-id="a3be4-139">Hogyan keresse meg a Storm event hub spout bináris fejlesztési</span><span class="sxs-lookup"><span data-stu-id="a3be4-139">How do I locate Storm event hub spout binaries for development</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-event-hub-spout-binaries-for-development)<br><br>[<span data-ttu-id="a3be4-140">Hogyan keresse meg a Storm Log4J konfigurációs fájlok fürtökön</span><span class="sxs-lookup"><span data-stu-id="a3be4-140">How do I locate Storm Log4J configuration files on clusters</span></span>](hdinsight-troubleshoot-storm.md#how-do-i-locate-storm-log4j-configuration-files-on-clusters)|
|<span data-ttu-id="a3be4-141">![YARN](./media/hdinsight-troubleshoot-guide/YARN.png)</span><span class="sxs-lookup"><span data-stu-id="a3be4-141">![YARN](./media/hdinsight-troubleshoot-guide/YARN.png)</span></span><br>[<span data-ttu-id="a3be4-142">YARN hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a3be4-142">Troubleshoot YARN</span></span>](hdinsight-troubleshoot-YARN.md)|<br>[<span data-ttu-id="a3be4-143">Hogyan hozzon létre egy új YARN várólistát egy fürtön</span><span class="sxs-lookup"><span data-stu-id="a3be4-143">How do I create a new YARN queue on a cluster</span></span>](hdinsight-troubleshoot-yarn.md#how-do-i-create-a-new-yarn-queue-on-a-cluster)<br><br>[<span data-ttu-id="a3be4-144">Hogyan le a YARN naplóit fürtből</span><span class="sxs-lookup"><span data-stu-id="a3be4-144">How do I download YARN logs from a cluster</span></span>](hdinsight-troubleshoot-yarn.md#how-do-i-download-yarn-logs-from-a-cluster)|

## <a name="hdinsight-troubleshooting-resources"></a><span data-ttu-id="a3be4-145">HDInsight-hibaelhárítási erőforrások</span><span class="sxs-lookup"><span data-stu-id="a3be4-145">HDInsight troubleshooting resources</span></span>

| <span data-ttu-id="a3be4-146">További információ</span><span class="sxs-lookup"><span data-stu-id="a3be4-146">For information about</span></span> | <span data-ttu-id="a3be4-147">Ezek a cikkek lásd:</span><span class="sxs-lookup"><span data-stu-id="a3be4-147">See these articles</span></span> |
| --- | --- |
| <span data-ttu-id="a3be4-148">HDInsight Linux-és optimalizálása</span><span class="sxs-lookup"><span data-stu-id="a3be4-148">HDInsight on Linux and optimization</span></span> | <span data-ttu-id="a3be4-149">- [Információ a HDInsight használata Linux rendszeren](hdinsight-hadoop-linux-information.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-149">- [Information about using HDInsight on Linux](hdinsight-hadoop-linux-information.md)</span></span><br><span data-ttu-id="a3be4-150">- [Hadoop memória és a teljesítmény hibaelhárítása](hdinsight-hadoop-stack-trace-error-messages.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-150">- [Hadoop memory and performance troubleshooting](hdinsight-hadoop-stack-trace-error-messages.md)</span></span><br><span data-ttu-id="a3be4-151">- [Hive-lekérdezések teljesítményét](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)</span><span class="sxs-lookup"><span data-stu-id="a3be4-151">- [Hive query performance](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)</span></span> |
| <span data-ttu-id="a3be4-152">Naplók és memóriaképek</span><span class="sxs-lookup"><span data-stu-id="a3be4-152">Logs and dumps</span></span> | <span data-ttu-id="a3be4-153">- [Linux bejelentkezik hozzáférést Hadoop YARN alkalmazás](hdinsight-hadoop-access-yarn-app-logs-linux.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-153">- [Access Hadoop YARN application logs on Linux](hdinsight-hadoop-access-yarn-app-logs-linux.md)</span></span><br><span data-ttu-id="a3be4-154">- [Halommemória memóriaképek Linux Hadoop-szolgáltatások engedélyezése](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-154">- [Enable heap dumps for Hadoop services on Linux](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span><br><span data-ttu-id="a3be4-155">- [HDInsight naplóinak elemzése](hdinsight-debug-jobs.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-155">- [Analyze HDInsight logs](hdinsight-debug-jobs.md)</span></span>|
| <span data-ttu-id="a3be4-156">Hibák</span><span class="sxs-lookup"><span data-stu-id="a3be4-156">Errors</span></span> | <span data-ttu-id="a3be4-157">- [Ismertetés és hárítsa el a WebHCat hibákat](hdinsight-hadoop-templeton-webhcat-debug-errors.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-157">- [Understand and resolve WebHCat errors](hdinsight-hadoop-templeton-webhcat-debug-errors.md)</span></span><br><span data-ttu-id="a3be4-158">- [Hive beállítások toofix OutofMemory hiba](hdinsight-hadoop-hive-out-of-memory-error-oom.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-158">- [Hive settings toofix OutofMemory error](hdinsight-hadoop-hive-out-of-memory-error-oom.md)</span></span> |
| <span data-ttu-id="a3be4-159">Eszközök</span><span class="sxs-lookup"><span data-stu-id="a3be4-159">Tools</span></span> | <span data-ttu-id="a3be4-160">- [Használja az Ambari nézetek toodebug Tez feladatokhoz](hdinsight-debug-ambari-tez-view.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-160">- [Use Ambari Views toodebug Tez jobs](hdinsight-debug-ambari-tez-view.md)</span></span><br><span data-ttu-id="a3be4-161">- [Hive-lekérdezések optimalizálása](hdinsight-hadoop-optimize-hive-query.md)</span><span class="sxs-lookup"><span data-stu-id="a3be4-161">- [Optimize Hive queries](hdinsight-hadoop-optimize-hive-query.md)</span></span> |