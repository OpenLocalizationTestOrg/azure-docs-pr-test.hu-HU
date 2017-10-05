---
title: "Azure HDInsight Hive hibaelhárításáról |} Microsoft Docs"
description: "Az Apache Hive és a Azure HDInsight kapcsolatos gyakori kérdésekre adott válaszok."
keywords: "Az Azure HDInsight Hive, gyakori kérdések hibaelhárítási útmutatója, gyakori kérdések"
services: Azure HDInsight
documentationcenter: na
author: dharmeshkakadia
manager: 
editor: 
ms.assetid: 15B8D0F3-F2D3-4746-BDCB-C72944AA9252
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: dharmeshkakadia
ms.openlocfilehash: 53e9685458190efe6a586504721b8e7baadaed60
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a><span data-ttu-id="2410a-104">Hive hibaelhárítása az Azure HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="2410a-104">Troubleshoot Hive by using Azure HDInsight</span></span>

<span data-ttu-id="2410a-105">A felső kérdések és azok megoldásait ismerje meg az Apache Ambari az Apache Hive Payload használatakor.</span><span class="sxs-lookup"><span data-stu-id="2410a-105">Learn about the top questions and their resolutions when working with Apache Hive payloads in Apache Ambari.</span></span>


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a><span data-ttu-id="2410a-106">Hogyan egy Hive metaadattárhoz exportálhatja és importálhatja azokat a fürt egy másik</span><span class="sxs-lookup"><span data-stu-id="2410a-106">How do I export a Hive metastore and import it on another cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="2410a-107">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="2410a-107">Resolution steps</span></span>

1. <span data-ttu-id="2410a-108">Csatlakozás egy Secure Shell (SSH) ügyfél segítségével a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="2410a-108">Connect to the HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="2410a-109">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="2410a-109">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="2410a-110">A következő parancsot a HDInsight-fürtre, amelyből el kívánja exportálni a metaadattárhoz:</span><span class="sxs-lookup"><span data-stu-id="2410a-110">Run the following command on the HDInsight cluster from which you want to export the metastore:</span></span>

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  <span data-ttu-id="2410a-111">A parancs létrehoz egy allatables.sql nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="2410a-111">This command generates a file named allatables.sql.</span></span>

3. <span data-ttu-id="2410a-112">A fájl alltables.sql másolja az új HDInsight-fürthöz, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2410a-112">Copy the file alltables.sql to the new HDInsight cluster, and then run the following command:</span></span>

  ```apache
  hive -f alltables.sql
  ```

<span data-ttu-id="2410a-113">A kódot a megoldási lépések feltételezi, hogy az új fürtön adatelérési utak ugyanaz, mint az adatelérési utak, a régi fürtön.</span><span class="sxs-lookup"><span data-stu-id="2410a-113">The code in the resolution steps assumes that data paths on the new cluster are the same as the data paths on the old cluster.</span></span> <span data-ttu-id="2410a-114">Ha az adatelérési utak nem egyezik, a létrehozott alltables.sql fájl módosult manuálisan módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2410a-114">If the data paths are different, you can manually edit the generated alltables.sql file to reflect any changes.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="2410a-115">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="2410a-115">Additional reading</span></span>

- [<span data-ttu-id="2410a-116">Csatlakozás egy HDInsight-fürthöz SSH használatával</span><span class="sxs-lookup"><span data-stu-id="2410a-116">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a><span data-ttu-id="2410a-117">Hogyan keresik meg Hive naplók fürt</span><span class="sxs-lookup"><span data-stu-id="2410a-117">How do I locate Hive logs on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="2410a-118">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="2410a-118">Resolution steps</span></span>

1. <span data-ttu-id="2410a-119">Kapcsolódás a HDInsight-fürthöz az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="2410a-119">Connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="2410a-120">További információkért lásd: **További olvasnivaló**.</span><span class="sxs-lookup"><span data-stu-id="2410a-120">For more information, see **Additional reading**.</span></span>

2. <span data-ttu-id="2410a-121">Hive ügyfél naplók megtekintéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="2410a-121">To view Hive client logs, use the following command:</span></span>

  ```apache
  /tmp/<username>/hive.log 
  ```

3. <span data-ttu-id="2410a-122">A következő paranccsal metaadattárhoz naplók Hive megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="2410a-122">To view Hive metastore logs, use the following command:</span></span>

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. <span data-ttu-id="2410a-123">Hiveserver naplók megtekintéséhez a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="2410a-123">To view Hiveserver logs, use the following command:</span></span>

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a><span data-ttu-id="2410a-124">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="2410a-124">Additional reading</span></span>

- [<span data-ttu-id="2410a-125">Csatlakozás egy HDInsight-fürthöz SSH használatával</span><span class="sxs-lookup"><span data-stu-id="2410a-125">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-the-hive-shell-with-specific-configurations-on-a-cluster"></a><span data-ttu-id="2410a-126">Hogyan indítsa el a fürt adott konfigurációjú a Hive rendszerhéjat</span><span class="sxs-lookup"><span data-stu-id="2410a-126">How do I launch the Hive shell with specific configurations on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="2410a-127">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="2410a-127">Resolution steps</span></span>

1. <span data-ttu-id="2410a-128">Adjon meg egy konfigurációs kulcs-érték párt, a Hive rendszerhéjat indításakor.</span><span class="sxs-lookup"><span data-stu-id="2410a-128">Specify a configuration key-value pair when you start the Hive shell.</span></span> <span data-ttu-id="2410a-129">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="2410a-129">For more information, see [Additional reading](#additional-reading-end).</span></span>

  ```apache
  hive -hiveconf a=b 
  ```

2. <span data-ttu-id="2410a-130">A Hive rendszerhéjat összes hatékony konfiguráció listájában, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2410a-130">To list all effective configurations on Hive shell, use the following command:</span></span>

  ```apache
  hive> set;
  ```

  <span data-ttu-id="2410a-131">Például az alábbi parancs segítségével indítsa el a Hive rendszerhéjat a hibakeresési naplózás engedélyezve van a konzolon:</span><span class="sxs-lookup"><span data-stu-id="2410a-131">For example, use the following command to start Hive shell with debug logging enabled on the console:</span></span>

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a><span data-ttu-id="2410a-132">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="2410a-132">Additional reading</span></span>

- [<span data-ttu-id="2410a-133">Hive konfiguráció tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="2410a-133">Hive configuration properties</span></span>](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <span data-ttu-id="2410a-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hogyan elemezheti a fürt kritikus útvonalra Tez DAG adatokat</span><span class="sxs-lookup"><span data-stu-id="2410a-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>How do I analyze Tez DAG data on a cluster-critical path</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="2410a-135">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="2410a-135">Resolution steps</span></span>
 
1. <span data-ttu-id="2410a-136">Elemezze az Apache Tez irányított aciklikus diagramhoz (DAG) egy fürt kritikus grafikonon, csatlakozzon a HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="2410a-136">To analyze an Apache Tez directed acyclic graph (DAG) on a cluster-critical graph, connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="2410a-137">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="2410a-137">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="2410a-138">A parancssorban futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2410a-138">At a command prompt, run the following command:</span></span>
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. <span data-ttu-id="2410a-139">Más lekérdezések, amelyek segítségével elemezheti a Tez DAG listájában, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2410a-139">To list other analyzers that can be used to analyze Tez DAG, use the following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  <span data-ttu-id="2410a-140">Meg kell adnia egy példa program első argumentumként.</span><span class="sxs-lookup"><span data-stu-id="2410a-140">You must provide an example program as the first argument.</span></span>

  <span data-ttu-id="2410a-141">A program érvényes nevek a következők:</span><span class="sxs-lookup"><span data-stu-id="2410a-141">Valid program names include:</span></span>
    - <span data-ttu-id="2410a-142">**ContainerReuseAnalyzer**: tároló újbóli részleteit egy DAG nyomtatása</span><span class="sxs-lookup"><span data-stu-id="2410a-142">**ContainerReuseAnalyzer**: Print container reuse details in a DAG</span></span>
    - <span data-ttu-id="2410a-143">**CriticalPath**: kritikus elérési útja egy adatbázis-rendelkezésreállási csoportok keresése</span><span class="sxs-lookup"><span data-stu-id="2410a-143">**CriticalPath**: Find the critical path of a DAG</span></span>
    - <span data-ttu-id="2410a-144">**LocalityAnalyzer**: egy DAG helység részleteket nyomtatása</span><span class="sxs-lookup"><span data-stu-id="2410a-144">**LocalityAnalyzer**: Print locality details in a DAG</span></span>
    - <span data-ttu-id="2410a-145">**ShuffleTimeAnalyzer**: elemzése a egy dag-csoport a véletlen idő – részletek</span><span class="sxs-lookup"><span data-stu-id="2410a-145">**ShuffleTimeAnalyzer**: Analyze the shuffle time details in a DAG</span></span>
    - <span data-ttu-id="2410a-146">**SkewAnalyzer**: egy DAG eltérésére részleteiről elemzése</span><span class="sxs-lookup"><span data-stu-id="2410a-146">**SkewAnalyzer**: Analyze the skew details in a DAG</span></span>
    - <span data-ttu-id="2410a-147">**SlowNodeAnalyzer**: csomópont részleteit egy DAG nyomtatása</span><span class="sxs-lookup"><span data-stu-id="2410a-147">**SlowNodeAnalyzer**: Print node details in a DAG</span></span>
    - <span data-ttu-id="2410a-148">**SlowTaskIdentifier**: egy dag-csoport a nyomtatás lassú feladat részletei</span><span class="sxs-lookup"><span data-stu-id="2410a-148">**SlowTaskIdentifier**: Print slow task details in a DAG</span></span>
    - <span data-ttu-id="2410a-149">**SlowestVertexAnalyzer**: egy DAG leglassabb csúcspont részleteket nyomtatása</span><span class="sxs-lookup"><span data-stu-id="2410a-149">**SlowestVertexAnalyzer**: Print slowest vertex details in a DAG</span></span>
    - <span data-ttu-id="2410a-150">**SpillAnalyzer**: nyomtatás spill részleteit egy dag-csoport</span><span class="sxs-lookup"><span data-stu-id="2410a-150">**SpillAnalyzer**: Print spill details in a DAG</span></span>
    - <span data-ttu-id="2410a-151">**TaskConcurrencyAnalyzer**: egy DAG párhuzamossági részlete nyomtatása</span><span class="sxs-lookup"><span data-stu-id="2410a-151">**TaskConcurrencyAnalyzer**: Print the task concurrency details in a DAG</span></span>
    - <span data-ttu-id="2410a-152">**VertexLevelCriticalPathAnalyzer**: a kritikus út csúcspont szinten található egy dag-csoport</span><span class="sxs-lookup"><span data-stu-id="2410a-152">**VertexLevelCriticalPathAnalyzer**: Find the critical path at vertex level in a DAG</span></span>


### <a name="additional-reading"></a><span data-ttu-id="2410a-153">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="2410a-153">Additional reading</span></span>

- [<span data-ttu-id="2410a-154">Csatlakozás egy HDInsight-fürthöz SSH használatával</span><span class="sxs-lookup"><span data-stu-id="2410a-154">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a><span data-ttu-id="2410a-155">Hogyan le Tez DAG adatok fürtből</span><span class="sxs-lookup"><span data-stu-id="2410a-155">How do I download Tez DAG data from a cluster</span></span>


#### <a name="resolution-steps"></a><span data-ttu-id="2410a-156">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="2410a-156">Resolution steps</span></span>

<span data-ttu-id="2410a-157">A Tez DAG adatok gyűjtéséhez két módja van:</span><span class="sxs-lookup"><span data-stu-id="2410a-157">There are two ways to collect the Tez DAG data:</span></span>

- <span data-ttu-id="2410a-158">A parancssorból:</span><span class="sxs-lookup"><span data-stu-id="2410a-158">From the command line:</span></span>
 
    <span data-ttu-id="2410a-159">Kapcsolódás a HDInsight-fürthöz az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="2410a-159">Connect to the HDInsight cluster by using SSH.</span></span> <span data-ttu-id="2410a-160">A parancssorban futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="2410a-160">At the command prompt, run the following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- <span data-ttu-id="2410a-161">Az Ambari Tez nézet használata:</span><span class="sxs-lookup"><span data-stu-id="2410a-161">Use the Ambari Tez view:</span></span>
   
  1. <span data-ttu-id="2410a-162">Ugrás az Ambari.</span><span class="sxs-lookup"><span data-stu-id="2410a-162">Go to Ambari.</span></span> 
  2. <span data-ttu-id="2410a-163">Ugrás a Tez nézetre (alatt a csempék ikonra, a jobb felső sarkában található).</span><span class="sxs-lookup"><span data-stu-id="2410a-163">Go to Tez view (under the tiles icon in the upper-right corner).</span></span> 
  3. <span data-ttu-id="2410a-164">Válassza ki a megtekinteni kívánt DAG.</span><span class="sxs-lookup"><span data-stu-id="2410a-164">Select the DAG you want to view.</span></span>
  4. <span data-ttu-id="2410a-165">Válassza ki **adatletöltéshez**.</span><span class="sxs-lookup"><span data-stu-id="2410a-165">Select **Download data**.</span></span>

### <span data-ttu-id="2410a-166"><a name="additional-reading-end"></a>További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="2410a-166"><a name="additional-reading-end"></a>Additional reading</span></span>

[<span data-ttu-id="2410a-167">Csatlakozás egy HDInsight-fürthöz SSH használatával</span><span class="sxs-lookup"><span data-stu-id="2410a-167">Connect to an HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)






