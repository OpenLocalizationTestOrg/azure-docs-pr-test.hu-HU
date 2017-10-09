---
title: "Hive aaaTroubleshoot Azure HDInsight használatával |} Microsoft Docs"
description: "Válaszok az Apache Hive és a Azure HDInsight toocommon kérdésekre."
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
ms.openlocfilehash: ac459316e658d0b29eb66f5685f0bc7e693bb277
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-hive-by-using-azure-hdinsight"></a><span data-ttu-id="b1e79-104">Hive hibaelhárítása az Azure HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="b1e79-104">Troubleshoot Hive by using Azure HDInsight</span></span>

<span data-ttu-id="b1e79-105">Hello felső kérdések és azok megoldásait ismerje meg az Apache Ambari az Apache Hive Payload használatakor.</span><span class="sxs-lookup"><span data-stu-id="b1e79-105">Learn about hello top questions and their resolutions when working with Apache Hive payloads in Apache Ambari.</span></span>


## <a name="how-do-i-export-a-hive-metastore-and-import-it-on-another-cluster"></a><span data-ttu-id="b1e79-106">Hogyan egy Hive metaadattárhoz exportálhatja és importálhatja azokat a fürt egy másik</span><span class="sxs-lookup"><span data-stu-id="b1e79-106">How do I export a Hive metastore and import it on another cluster</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b1e79-107">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="b1e79-107">Resolution steps</span></span>

1. <span data-ttu-id="b1e79-108">Toohello HDInsight-fürtjéhez Secure Shell (SSH) ügyfél csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="b1e79-108">Connect toohello HDInsight cluster by using a Secure Shell (SSH) client.</span></span> <span data-ttu-id="b1e79-109">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="b1e79-109">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="b1e79-110">Futtassa a parancsot követő hello HDInsight-fürtre, amelyből el kívánja tooexport hello metaadattárhoz hello:</span><span class="sxs-lookup"><span data-stu-id="b1e79-110">Run hello following command on hello HDInsight cluster from which you want tooexport hello metastore:</span></span>

    ```apache
    for d in `hive -e "show databases"`; do echo "create database $d; use $d;" >> alltables.sql ; for t in `hive --database $d -e "show tables"` ; do ddl=`hive --database $d -e "show create table $t"`; echo "$ddl ;" >> alltables.sql ; echo "$ddl" | grep -q "PARTITIONED\s*BY" && echo "MSCK REPAIR TABLE $t ;" >> alltables.sql ; done; done
    ```

  <span data-ttu-id="b1e79-111">A parancs létrehoz egy allatables.sql nevű fájlt.</span><span class="sxs-lookup"><span data-stu-id="b1e79-111">This command generates a file named allatables.sql.</span></span>

3. <span data-ttu-id="b1e79-112">Hello fájl alltables.sql toohello új HDInsight-fürt másolja, és futtassa a parancsot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b1e79-112">Copy hello file alltables.sql toohello new HDInsight cluster, and then run hello following command:</span></span>

  ```apache
  hive -f alltables.sql
  ```

<span data-ttu-id="b1e79-113">hello kód hello megoldási lépések a azt feltételezi, hogy hello új fürtön az elérési utakat adatok hello azonos hello adatelérési utak hello régi fürtön.</span><span class="sxs-lookup"><span data-stu-id="b1e79-113">hello code in hello resolution steps assumes that data paths on hello new cluster are hello same as hello data paths on hello old cluster.</span></span> <span data-ttu-id="b1e79-114">Ha hello adatelérési utak eltérőek, manuálisan módosíthatja generált hello alltables.sql fájl tooreflect módosításokat.</span><span class="sxs-lookup"><span data-stu-id="b1e79-114">If hello data paths are different, you can manually edit hello generated alltables.sql file tooreflect any changes.</span></span>

### <a name="additional-reading"></a><span data-ttu-id="b1e79-115">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b1e79-115">Additional reading</span></span>

- [<span data-ttu-id="b1e79-116">Csatlakozás tooan HDInsight-fürthöz SSH segítségével</span><span class="sxs-lookup"><span data-stu-id="b1e79-116">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-locate-hive-logs-on-a-cluster"></a><span data-ttu-id="b1e79-117">Hogyan keresik meg Hive naplók fürt</span><span class="sxs-lookup"><span data-stu-id="b1e79-117">How do I locate Hive logs on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="b1e79-118">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="b1e79-118">Resolution steps</span></span>

1. <span data-ttu-id="b1e79-119">Csatlakozás toohello HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="b1e79-119">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="b1e79-120">További információkért lásd: **További olvasnivaló**.</span><span class="sxs-lookup"><span data-stu-id="b1e79-120">For more information, see **Additional reading**.</span></span>

2. <span data-ttu-id="b1e79-121">tooview Hive ügyfeleinek naplói, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="b1e79-121">tooview Hive client logs, use hello following command:</span></span>

  ```apache
  /tmp/<username>/hive.log 
  ```

3. <span data-ttu-id="b1e79-122">tooview Hive metaadattárhoz naplókat, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="b1e79-122">tooview Hive metastore logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hivemetastore.log 
  ```

4. <span data-ttu-id="b1e79-123">tooview Hiveserver naplókat, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="b1e79-123">tooview Hiveserver logs, use hello following command:</span></span>

  ```apache
  /var/log/hive/hiveserver2.log 
  ```

### <a name="additional-reading"></a><span data-ttu-id="b1e79-124">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b1e79-124">Additional reading</span></span>

- [<span data-ttu-id="b1e79-125">Csatlakozás tooan HDInsight-fürthöz SSH segítségével</span><span class="sxs-lookup"><span data-stu-id="b1e79-125">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-launch-hello-hive-shell-with-specific-configurations-on-a-cluster"></a><span data-ttu-id="b1e79-126">Hogyan indítsa el a Hive rendszerhéjat egy fürtön adott konfigurációjú hello</span><span class="sxs-lookup"><span data-stu-id="b1e79-126">How do I launch hello Hive shell with specific configurations on a cluster</span></span>

### <a name="resolution-steps"></a><span data-ttu-id="b1e79-127">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="b1e79-127">Resolution steps</span></span>

1. <span data-ttu-id="b1e79-128">Adjon meg egy konfigurációs kulcs-érték párt, hello Hive rendszerhéjat indításakor.</span><span class="sxs-lookup"><span data-stu-id="b1e79-128">Specify a configuration key-value pair when you start hello Hive shell.</span></span> <span data-ttu-id="b1e79-129">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="b1e79-129">For more information, see [Additional reading](#additional-reading-end).</span></span>

  ```apache
  hive -hiveconf a=b 
  ```

2. <span data-ttu-id="b1e79-130">toolist összes hatékony konfiguráció a Hive rendszerhéjat, használjon hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b1e79-130">toolist all effective configurations on Hive shell, use hello following command:</span></span>

  ```apache
  hive> set;
  ```

  <span data-ttu-id="b1e79-131">Például használhatja hello toostart Hive parancshéj hibakeresési naplózással hello konzol engedélyezve a következő:</span><span class="sxs-lookup"><span data-stu-id="b1e79-131">For example, use hello following command toostart Hive shell with debug logging enabled on hello console:</span></span>

  ```apache
  hive -hiveconf hive.root.logger=ALL,console 
  ```

### <a name="additional-reading"></a><span data-ttu-id="b1e79-132">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b1e79-132">Additional reading</span></span>

- [<span data-ttu-id="b1e79-133">Hive konfiguráció tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b1e79-133">Hive configuration properties</span></span>](https://cwiki.apache.org/confluence/display/Hive/Configuration+Properties)


## <span data-ttu-id="b1e79-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>Hogyan elemezheti a fürt kritikus útvonalra Tez DAG adatokat</span><span class="sxs-lookup"><span data-stu-id="b1e79-134"><a name="how-do-i-analyze-tez-dag-data-on-a-cluster-critical-path"></a>How do I analyze Tez DAG data on a cluster-critical path</span></span>


### <a name="resolution-steps"></a><span data-ttu-id="b1e79-135">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="b1e79-135">Resolution steps</span></span>
 
1. <span data-ttu-id="b1e79-136">az Apache Tez tooanalyze irányított aciklikus diagramhoz (DAG) egy fürt kritikus grafikonon, toohello HDInsight-fürt csatlakozzon az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="b1e79-136">tooanalyze an Apache Tez directed acyclic graph (DAG) on a cluster-critical graph, connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="b1e79-137">További információkért lásd: [További olvasnivaló](#additional-reading-end).</span><span class="sxs-lookup"><span data-stu-id="b1e79-137">For more information, see [Additional reading](#additional-reading-end).</span></span>

2. <span data-ttu-id="b1e79-138">Parancsot egy parancssorba futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b1e79-138">At a command prompt, run hello following command:</span></span>
   
  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar CriticalPath --saveResults --dagId <DagId> --eventFileName <DagData.zip> 
  ```

3. <span data-ttu-id="b1e79-139">toolist más lehet foglalt tooanalyze Tez DAG elemzőkkel hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="b1e79-139">toolist other analyzers that can be used tooanalyze Tez DAG, use hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-job-analyzer-*.jar
  ```

  <span data-ttu-id="b1e79-140">Meg kell adnia egy példa program hello első argumentumként.</span><span class="sxs-lookup"><span data-stu-id="b1e79-140">You must provide an example program as hello first argument.</span></span>

  <span data-ttu-id="b1e79-141">A program érvényes nevek a következők:</span><span class="sxs-lookup"><span data-stu-id="b1e79-141">Valid program names include:</span></span>
    - <span data-ttu-id="b1e79-142">**ContainerReuseAnalyzer**: tároló újbóli részleteit egy DAG nyomtatása</span><span class="sxs-lookup"><span data-stu-id="b1e79-142">**ContainerReuseAnalyzer**: Print container reuse details in a DAG</span></span>
    - <span data-ttu-id="b1e79-143">**CriticalPath**: keresés hello kritikus elérési útja egy dag-csoport</span><span class="sxs-lookup"><span data-stu-id="b1e79-143">**CriticalPath**: Find hello critical path of a DAG</span></span>
    - <span data-ttu-id="b1e79-144">**LocalityAnalyzer**: egy DAG helység részleteket nyomtatása</span><span class="sxs-lookup"><span data-stu-id="b1e79-144">**LocalityAnalyzer**: Print locality details in a DAG</span></span>
    - <span data-ttu-id="b1e79-145">**ShuffleTimeAnalyzer**: hello sorrendű idő – részletek a egy DAG elemzése</span><span class="sxs-lookup"><span data-stu-id="b1e79-145">**ShuffleTimeAnalyzer**: Analyze hello shuffle time details in a DAG</span></span>
    - <span data-ttu-id="b1e79-146">**SkewAnalyzer**: hello eltérésére részleteket a egy dag-csoport</span><span class="sxs-lookup"><span data-stu-id="b1e79-146">**SkewAnalyzer**: Analyze hello skew details in a DAG</span></span>
    - <span data-ttu-id="b1e79-147">**SlowNodeAnalyzer**: csomópont részleteit egy DAG nyomtatása</span><span class="sxs-lookup"><span data-stu-id="b1e79-147">**SlowNodeAnalyzer**: Print node details in a DAG</span></span>
    - <span data-ttu-id="b1e79-148">**SlowTaskIdentifier**: egy dag-csoport a nyomtatás lassú feladat részletei</span><span class="sxs-lookup"><span data-stu-id="b1e79-148">**SlowTaskIdentifier**: Print slow task details in a DAG</span></span>
    - <span data-ttu-id="b1e79-149">**SlowestVertexAnalyzer**: egy DAG leglassabb csúcspont részleteket nyomtatása</span><span class="sxs-lookup"><span data-stu-id="b1e79-149">**SlowestVertexAnalyzer**: Print slowest vertex details in a DAG</span></span>
    - <span data-ttu-id="b1e79-150">**SpillAnalyzer**: nyomtatás spill részleteit egy dag-csoport</span><span class="sxs-lookup"><span data-stu-id="b1e79-150">**SpillAnalyzer**: Print spill details in a DAG</span></span>
    - <span data-ttu-id="b1e79-151">**TaskConcurrencyAnalyzer**: hello feladat egyidejű részleteit egy DAG nyomtatása</span><span class="sxs-lookup"><span data-stu-id="b1e79-151">**TaskConcurrencyAnalyzer**: Print hello task concurrency details in a DAG</span></span>
    - <span data-ttu-id="b1e79-152">**VertexLevelCriticalPathAnalyzer**: Find hello kritikus elérési út egy DAG csúcspont szinten</span><span class="sxs-lookup"><span data-stu-id="b1e79-152">**VertexLevelCriticalPathAnalyzer**: Find hello critical path at vertex level in a DAG</span></span>


### <a name="additional-reading"></a><span data-ttu-id="b1e79-153">További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b1e79-153">Additional reading</span></span>

- [<span data-ttu-id="b1e79-154">Csatlakozás tooan HDInsight-fürthöz SSH segítségével</span><span class="sxs-lookup"><span data-stu-id="b1e79-154">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)


## <a name="how-do-i-download-tez-dag-data-from-a-cluster"></a><span data-ttu-id="b1e79-155">Hogyan le Tez DAG adatok fürtből</span><span class="sxs-lookup"><span data-stu-id="b1e79-155">How do I download Tez DAG data from a cluster</span></span>


#### <a name="resolution-steps"></a><span data-ttu-id="b1e79-156">Megoldási lépések</span><span class="sxs-lookup"><span data-stu-id="b1e79-156">Resolution steps</span></span>

<span data-ttu-id="b1e79-157">Két módon toocollect hello Tez DAG adatokat:</span><span class="sxs-lookup"><span data-stu-id="b1e79-157">There are two ways toocollect hello Tez DAG data:</span></span>

- <span data-ttu-id="b1e79-158">Hello parancssorból:</span><span class="sxs-lookup"><span data-stu-id="b1e79-158">From hello command line:</span></span>
 
    <span data-ttu-id="b1e79-159">Csatlakozás toohello HDInsight-fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="b1e79-159">Connect toohello HDInsight cluster by using SSH.</span></span> <span data-ttu-id="b1e79-160">Hello parancssorban futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="b1e79-160">At hello command prompt, run hello following command:</span></span>

  ```apache
  hadoop jar /usr/hdp/current/tez-client/tez-history-parser-*.jar org.apache.tez.history.ATSImportTool -downloadDir . -dagId <DagId> 
  ```

- <span data-ttu-id="b1e79-161">Ambari Tez nézet hello használata:</span><span class="sxs-lookup"><span data-stu-id="b1e79-161">Use hello Ambari Tez view:</span></span>
   
  1. <span data-ttu-id="b1e79-162">Nyissa meg tooAmbari.</span><span class="sxs-lookup"><span data-stu-id="b1e79-162">Go tooAmbari.</span></span> 
  2. <span data-ttu-id="b1e79-163">(A hello csempék ikon hello jobb felső sarkában található) lépjen tooTez nézet.</span><span class="sxs-lookup"><span data-stu-id="b1e79-163">Go tooTez view (under hello tiles icon in hello upper-right corner).</span></span> 
  3. <span data-ttu-id="b1e79-164">Válassza ki a kívánt tooview DAG hello.</span><span class="sxs-lookup"><span data-stu-id="b1e79-164">Select hello DAG you want tooview.</span></span>
  4. <span data-ttu-id="b1e79-165">Válassza ki **adatletöltéshez**.</span><span class="sxs-lookup"><span data-stu-id="b1e79-165">Select **Download data**.</span></span>

### <span data-ttu-id="b1e79-166"><a name="additional-reading-end"></a>További olvasnivaló</span><span class="sxs-lookup"><span data-stu-id="b1e79-166"><a name="additional-reading-end"></a>Additional reading</span></span>

[<span data-ttu-id="b1e79-167">Csatlakozás tooan HDInsight-fürthöz SSH segítségével</span><span class="sxs-lookup"><span data-stu-id="b1e79-167">Connect tooan HDInsight cluster by using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)






