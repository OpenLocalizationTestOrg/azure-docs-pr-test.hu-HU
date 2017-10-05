---
title: "Azure HDInsight alatt futó Storm hibaelhárításáról |} Microsoft Docs"
description: "Az Azure HDInsight alatt futó Apache Storm használatával kapcsolatos gyakori kérdésekre adott válaszok."
keywords: "Az Azure HDInsight alatt futó Storm, gyakori kérdések hibaelhárítási útmutatója, gyakori problémák"
services: Azure HDInsight
documentationcenter: na
author: raviperi
manager: 
editor: 
ms.assetid: 74E51183-3EF4-4C67-AA60-6E12FAC999B5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/7/2017
ms.author: raviperi
ms.openlocfilehash: 70a3d762431d90acdd6ed2a432a569f34d0ce447
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="00251-104">Azure HDInsight alatt futó Storm hibaelhárításáról</span><span class="sxs-lookup"><span data-stu-id="00251-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="00251-105">További tudnivalók a legfőbb problémákat és azok megoldásait Apache Ambari az Apache Storm hasznos adatot való munkához.</span><span class="sxs-lookup"><span data-stu-id="00251-105">Learn about the top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-the-storm-ui-on-a-cluster"></a><span data-ttu-id="00251-106">Hogyan érhetem el az egy fürtön a Storm felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="00251-106">How do I access the Storm UI on a cluster</span></span>
<span data-ttu-id="00251-107">A Storm felhasználói felülete eléréséhez a böngészőben két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="00251-107">You have two options for accessing the Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="00251-108">Ambari felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="00251-108">Ambari UI</span></span>
1. <span data-ttu-id="00251-109">Nyissa meg az Ambari irányítópultot.</span><span class="sxs-lookup"><span data-stu-id="00251-109">Go to the Ambari dashboard.</span></span>
2. <span data-ttu-id="00251-110">A szolgáltatások listájában jelölje ki **Storm**.</span><span class="sxs-lookup"><span data-stu-id="00251-110">In the list of services, select **Storm**.</span></span>
3. <span data-ttu-id="00251-111">Az a **Gyorshivatkozások** menü **Storm felhasználói felülete**.</span><span class="sxs-lookup"><span data-stu-id="00251-111">In the **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="00251-112">A közvetlen hivatkozás</span><span class="sxs-lookup"><span data-stu-id="00251-112">Direct link</span></span>
<span data-ttu-id="00251-113">A Storm felhasználói felülete a következő URL-címen érhető el:</span><span class="sxs-lookup"><span data-stu-id="00251-113">You can access the Storm UI at the following URL:</span></span>

<span data-ttu-id="00251-114">https://\<fürt DNS-név\>/stormui</span><span class="sxs-lookup"><span data-stu-id="00251-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="00251-115">Példa:</span><span class="sxs-lookup"><span data-stu-id="00251-115">Example:</span></span>

 <span data-ttu-id="00251-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="00251-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-to-another"></a><span data-ttu-id="00251-117">Hogyan tegye I átvitele Storm hub spout ellenőrzőpont eseményadatok egy topológia között</span><span class="sxs-lookup"><span data-stu-id="00251-117">How do I transfer Storm event hub spout checkpoint information from one topology to another</span></span>

<span data-ttu-id="00251-118">Amikor olvassa el az Azure Event Hubs topológiák fejlesztése a HDInsight alatt futó Storm eseményközpont használatával spout található .jar fájl, telepítenie kell egy új fürt meg a névvel rendelkező topológia.</span><span class="sxs-lookup"><span data-stu-id="00251-118">When you develop topologies that read from Azure Event Hubs by using the HDInsight Storm event hub spout .jar file, you must deploy a topology that has the same name on a new cluster.</span></span> <span data-ttu-id="00251-119">Azonban meg kell őrizni, hogy a régi fürtön Apache ZooKeeper lett véglegesített ellenőrzőpont-adatok.</span><span class="sxs-lookup"><span data-stu-id="00251-119">However, you must retain the checkpoint data that was committed to Apache ZooKeeper on the old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="00251-120">Ellenőrzőpont-adatok tárolására</span><span class="sxs-lookup"><span data-stu-id="00251-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="00251-121">Az eltolások ellenőrzőpont adatait a event hub spout, ZooKeeper a két legfelső szintű elérési útjában tárolja:</span><span class="sxs-lookup"><span data-stu-id="00251-121">Checkpoint data for offsets is stored by the event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="00251-122">Nem tranzakciós spout ellenőrzőpontokat /eventhubspout vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="00251-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="00251-123">Be- / tranzakciós tranzakciós spout ellenőrzőpont adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="00251-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-to-restore"></a><span data-ttu-id="00251-124">Visszaállítása</span><span class="sxs-lookup"><span data-stu-id="00251-124">How to restore</span></span>
<span data-ttu-id="00251-125">Ahhoz, hogy a parancsfájlok és a szalagtárak, amelyekkel ZooKeeper kívüli adatok exportálása és majd importálja vissza az adatokat egy új nevű ZooKeeper, lásd: [HDInsight alatt futó Storm példák](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="00251-125">To get the scripts and libraries that you use to export data out of ZooKeeper and then import the data back to ZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="00251-126">A lib mappa rendelkezik .jar fájlok, amelyek tartalmazzák az exportálási/importálási művelet végrehajtására.</span><span class="sxs-lookup"><span data-stu-id="00251-126">The lib folder has .jar files that contain the implementation for the export/import operation.</span></span> <span data-ttu-id="00251-127">A bash mappa rendelkezik egy példa a parancsfájl azt mutatja be, és adatok exportálása a ZooKeeper-kiszolgáló a régi fürtön, majd importálja vissza a ZooKeeper server az új fürtön.</span><span class="sxs-lookup"><span data-stu-id="00251-127">The bash folder has an example script that demonstrates how to export data from the ZooKeeper server on the old cluster, and then import it back to the ZooKeeper server on the new cluster.</span></span>

<span data-ttu-id="00251-128">Futtassa a [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) exportálása, majd az adatok importálása a ZooKeeper csomópontok parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="00251-128">Run the [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from the ZooKeeper nodes to export and then import data.</span></span> <span data-ttu-id="00251-129">Frissítse a parancsfájlt a megfelelő Hortonworks Data Platform (HDP) verzióra.</span><span class="sxs-lookup"><span data-stu-id="00251-129">Update the script to the correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="00251-130">(Jelenleg is dolgozunk a parancsfájlokat a Hdinsightban általános elvégzése.</span><span class="sxs-lookup"><span data-stu-id="00251-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="00251-131">Az általános parancsfájlok bármely olyan csomópontról módosítások nélkül a fürtön a felhasználó futtathatja.)</span><span class="sxs-lookup"><span data-stu-id="00251-131">Generic scripts can run from any node on the cluster without modifications by the user.)</span></span>

<span data-ttu-id="00251-132">A parancs a metaadatok ír egy Apache Hadoop elosztott fájlrendszerrel (HDFS) útvonal (az Azure Blob Storage vagy az Azure Data Lake Store áruházban), amely egy helyen.</span><span class="sxs-lookup"><span data-stu-id="00251-132">The export command writes the metadata to an Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="00251-133">Példák</span><span class="sxs-lookup"><span data-stu-id="00251-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="00251-134">Az eltolási metaadatok exportálása</span><span class="sxs-lookup"><span data-stu-id="00251-134">Export offset metadata</span></span>
1. <span data-ttu-id="00251-135">SSH segítségével nyissa meg a fürt, amelyből az ellenőrzőpont eltolás exportálni kell a ZooKeeper fürt.</span><span class="sxs-lookup"><span data-stu-id="00251-135">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="00251-136">A következő parancsot (miután frissítette a HDP verzió-karakterlánca) ZooKeeper eltolási adatainak exportálása a /stormmetadta/zkdata HDFS elérési útja:</span><span class="sxs-lookup"><span data-stu-id="00251-136">Run the following command (after you update the HDP version string) to export ZooKeeper offset data to the /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="00251-137">Az eltolási metaadatok importálása</span><span class="sxs-lookup"><span data-stu-id="00251-137">Import offset metadata</span></span>
1. <span data-ttu-id="00251-138">SSH segítségével nyissa meg a fürt, amelyből az ellenőrzőpont eltolás exportálni kell a ZooKeeper fürt.</span><span class="sxs-lookup"><span data-stu-id="00251-138">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="00251-139">A következő parancsot (miután frissítette a HDP verzió-karakterlánca) ZooKeeper eltolási adatok importálása a HDFS elérési /stormmetadata/zkdata a ZooKeeper-kiszolgáló a célfürtön:</span><span class="sxs-lookup"><span data-stu-id="00251-139">Run the following command (after you update the HDP version string) to import ZooKeeper offset data from the HDFS path /stormmetadata/zkdata to the ZooKeeper server on the target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-the-beginning-or-from-a-timestamp-that-the-user-chooses"></a><span data-ttu-id="00251-140">Így a topológiák adatainak feldolgozása elindíthatja az elejéről, vagy a felhasználó által kiválasztott időbélyeg eltolási metaadatok törlése</span><span class="sxs-lookup"><span data-stu-id="00251-140">Delete offset metadata so that topologies can start processing data from the beginning, or from a timestamp that the user chooses</span></span>
1. <span data-ttu-id="00251-141">SSH segítségével nyissa meg a fürt, amelyből az ellenőrzőpont eltolás exportálni kell a ZooKeeper fürt.</span><span class="sxs-lookup"><span data-stu-id="00251-141">Use SSH to go to the ZooKeeper cluster on the cluster from which the checkpoint offset needs to be exported.</span></span>
2. <span data-ttu-id="00251-142">A következő parancsot (HDP verzió-karakterlánca frissítése) után törli az összes ZooKeeper eltolási adatokat az aktuális fürt:</span><span class="sxs-lookup"><span data-stu-id="00251-142">Run the following command (after you update the HDP version string) to delete all ZooKeeper offset data in the current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="00251-143">Hogyan keresse meg a Storm bináris fürt</span><span class="sxs-lookup"><span data-stu-id="00251-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="00251-144">Az aktuális HDP verem Storm bináris /usr/hdp/current/storm-client szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="00251-144">Storm binaries for the current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="00251-145">A hely az azonos átjárócsomópontokkal és munkavégző csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="00251-145">The location is the same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="00251-146">Előfordulhat, hogy több bináris fájljait (például /usr/hdp/2.5.0.1233/storm) /usr/hdp adott HDP verzióihoz.</span><span class="sxs-lookup"><span data-stu-id="00251-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="00251-147">A /usr/hdp/current/storm-client mappa nem symlinked a legújabb verzióra, hogy fut a fürtön.</span><span class="sxs-lookup"><span data-stu-id="00251-147">The /usr/hdp/current/storm-client folder is symlinked to the latest version that is running on the cluster.</span></span>

<span data-ttu-id="00251-148">További információkért lásd: [egy HDInsight-fürthöz SSH használatával csatlakozhat](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="00251-148">For more information, see [Connect to an HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-the-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="00251-149">Hogyan állapítható meg a telepítési topológia a Storm-fürt</span><span class="sxs-lookup"><span data-stu-id="00251-149">How do I determine the deployment topology of a Storm cluster</span></span>
<span data-ttu-id="00251-150">Először azonosítsa a HDInsight alatt futó Storm, telepített összetevők.</span><span class="sxs-lookup"><span data-stu-id="00251-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="00251-151">A Storm-fürt négy csomópont kategóriák áll:</span><span class="sxs-lookup"><span data-stu-id="00251-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="00251-152">Átjárócsomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-152">Gateway nodes</span></span>
* <span data-ttu-id="00251-153">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-153">Head nodes</span></span>
* <span data-ttu-id="00251-154">ZooKeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="00251-155">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="00251-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="00251-156">Átjárócsomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-156">Gateway nodes</span></span>
<span data-ttu-id="00251-157">Átjáró csomópont egy átjáró és a fordított proxy szolgáltatás, amely lehetővé teszi egy aktív Ambari felügyeleti szolgáltatás való nyilvános hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="00251-157">A gateway node is a gateway and reverse proxy service that enables public access to an active Ambari management service.</span></span> <span data-ttu-id="00251-158">Azt is kezeli az Ambari vezető választás.</span><span class="sxs-lookup"><span data-stu-id="00251-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="00251-159">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-159">Head nodes</span></span>
<span data-ttu-id="00251-160">A Storm átjárócsomópontokkal futtassa a következő szolgáltatásokat:</span><span class="sxs-lookup"><span data-stu-id="00251-160">Storm head nodes run the following services:</span></span>
* <span data-ttu-id="00251-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="00251-161">Nimbus</span></span>
* <span data-ttu-id="00251-162">Ambari kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="00251-162">Ambari server</span></span>
* <span data-ttu-id="00251-163">Ambari metrikák kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="00251-163">Ambari Metrics server</span></span>
* <span data-ttu-id="00251-164">Ambari metrikákat gyűjtő</span><span class="sxs-lookup"><span data-stu-id="00251-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="00251-165">ZooKeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-165">ZooKeeper nodes</span></span>
<span data-ttu-id="00251-166">HDInsight egy három csomópontos ZooKeeper kvórum tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="00251-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="00251-167">A kvórum mérete rögzített, és nem kell újrakonfigurálni.</span><span class="sxs-lookup"><span data-stu-id="00251-167">The quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="00251-168">Storm szolgáltatás a fürt automatikusan a ZooKeeper kvórum használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="00251-168">Storm services in the cluster are configured to automatically use the ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="00251-169">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="00251-169">Worker nodes</span></span>
<span data-ttu-id="00251-170">A Storm munkavégző csomópontokhoz futtassa a következő szolgáltatásokat:</span><span class="sxs-lookup"><span data-stu-id="00251-170">Storm worker nodes run the following services:</span></span>
* <span data-ttu-id="00251-171">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="00251-171">Supervisor</span></span>
* <span data-ttu-id="00251-172">Munkavégző Java virtuális gépek (JVMs), a futó topológiák</span><span class="sxs-lookup"><span data-stu-id="00251-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="00251-173">Ambari ügynök</span><span class="sxs-lookup"><span data-stu-id="00251-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="00251-174">Hogyan keresse meg a Storm event hub spout bináris fejlesztési</span><span class="sxs-lookup"><span data-stu-id="00251-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="00251-175">A topológia a Storm event hub spout .jar fájlok használatával kapcsolatos további információkért lásd a következőket.</span><span class="sxs-lookup"><span data-stu-id="00251-175">For more information about using Storm event hub spout .jar files with your topology, see the following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="00251-176">Java-alapú topológia</span><span class="sxs-lookup"><span data-stu-id="00251-176">Java-based topology</span></span>
[<span data-ttu-id="00251-177">Az Azure Event Hubs (Java) futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="00251-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="00251-178">C#-alapú topológia (a HDInsight 3.4 + Linux Storm-fürtök monó)</span><span class="sxs-lookup"><span data-stu-id="00251-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
[<span data-ttu-id="00251-179">Az Azure Event Hubs-eseményközpontok eseményeinek feldolgozása a HDInsight alatt futó Stormmal (C#)</span><span class="sxs-lookup"><span data-stu-id="00251-179">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="00251-180">Legújabb Storm event hub spout binárist 3.5 + HDInsight Linux Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="00251-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="00251-181">A legújabb Storm event hub spout, amely kompatibilis a HDInsight 3.5 + Linux Storm-fürtök használatával kapcsolatban lásd: a mvn-tárház [információs fájl](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="00251-181">To learn how to use the latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see the mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="00251-182">Forrás-kódpéldák</span><span class="sxs-lookup"><span data-stu-id="00251-182">Source code examples</span></span>
<span data-ttu-id="00251-183">Lásd: [példák](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) bemutatja, hogyan olvashatja és írhatja az Azure Event Hubs egy Apache Storm-topológia (Java nyelven írt) használata az Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="00251-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how to read and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="00251-184">Hogyan keresse meg a Storm Log4J konfigurációs fájlok fürtökön</span><span class="sxs-lookup"><span data-stu-id="00251-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="00251-185">Apache Log4J konfigurációs fájlok Storm szolgáltatások azonosításához.</span><span class="sxs-lookup"><span data-stu-id="00251-185">To identify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="00251-186">Az átjárócsomópontokkal</span><span class="sxs-lookup"><span data-stu-id="00251-186">On head nodes</span></span>
<span data-ttu-id="00251-187">A Nimbus Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="00251-187">The Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="00251-188">A feldolgozó csomópontok</span><span class="sxs-lookup"><span data-stu-id="00251-188">On worker nodes</span></span>
<span data-ttu-id="00251-189">A felügyelő Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="00251-189">The supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="00251-190">A munkavégző Log4J konfigurációs fájl olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="00251-190">The worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="00251-191">Példák: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="00251-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

