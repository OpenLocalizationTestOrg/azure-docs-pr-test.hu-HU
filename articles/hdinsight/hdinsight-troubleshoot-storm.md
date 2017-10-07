---
title: Azure HDInsight a Storm aaaTroubleshoot |} Microsoft Docs
description: "Válaszok toocommon kérdések Azure HDInsight alatt futó Apache Storm használatával kapcsolatban."
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
ms.openlocfilehash: 51bcb3dc28eff5ee7bb33252fb2ec71a88ed8e09
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-storm-by-using-azure-hdinsight"></a><span data-ttu-id="055a3-104">Azure HDInsight alatt futó Storm hibaelhárításáról</span><span class="sxs-lookup"><span data-stu-id="055a3-104">Troubleshoot Storm by using Azure HDInsight</span></span>

<span data-ttu-id="055a3-105">További tudnivalók a hello legfőbb problémákat és azok megoldásait Apache Ambari az Apache Storm hasznos adatot való munkához.</span><span class="sxs-lookup"><span data-stu-id="055a3-105">Learn about hello top issues and their resolutions for working with Apache Storm payloads in Apache Ambari.</span></span>

## <a name="how-do-i-access-hello-storm-ui-on-a-cluster"></a><span data-ttu-id="055a3-106">Hogyan érhetem el az egy fürtön található Storm felhasználói felülete hello</span><span class="sxs-lookup"><span data-stu-id="055a3-106">How do I access hello Storm UI on a cluster</span></span>
<span data-ttu-id="055a3-107">Hello Storm felhasználói felülete eléréséhez a böngészőben két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="055a3-107">You have two options for accessing hello Storm UI from a browser:</span></span>

### <a name="ambari-ui"></a><span data-ttu-id="055a3-108">Ambari felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="055a3-108">Ambari UI</span></span>
1. <span data-ttu-id="055a3-109">Nyissa meg toohello Ambari irányítópult.</span><span class="sxs-lookup"><span data-stu-id="055a3-109">Go toohello Ambari dashboard.</span></span>
2. <span data-ttu-id="055a3-110">Válassza a szolgáltatások hello lista **Storm**.</span><span class="sxs-lookup"><span data-stu-id="055a3-110">In hello list of services, select **Storm**.</span></span>
3. <span data-ttu-id="055a3-111">A hello **Gyorshivatkozások** menü **Storm felhasználói felülete**.</span><span class="sxs-lookup"><span data-stu-id="055a3-111">In hello **Quick Links** menu, select **Storm UI**.</span></span>

### <a name="direct-link"></a><span data-ttu-id="055a3-112">A közvetlen hivatkozás</span><span class="sxs-lookup"><span data-stu-id="055a3-112">Direct link</span></span>
<span data-ttu-id="055a3-113">Hello Storm felhasználói felülete hello URL-cím a következő címen érhető el:</span><span class="sxs-lookup"><span data-stu-id="055a3-113">You can access hello Storm UI at hello following URL:</span></span>

<span data-ttu-id="055a3-114">https://\<fürt DNS-név\>/stormui</span><span class="sxs-lookup"><span data-stu-id="055a3-114">https://\<cluster DNS name\>/stormui</span></span>

<span data-ttu-id="055a3-115">Példa:</span><span class="sxs-lookup"><span data-stu-id="055a3-115">Example:</span></span>

 <span data-ttu-id="055a3-116">https://stormcluster.azurehdinsight.NET/stormui</span><span class="sxs-lookup"><span data-stu-id="055a3-116">https://stormcluster.azurehdinsight.net/stormui</span></span>

## <a name="how-do-i-transfer-storm-event-hub-spout-checkpoint-information-from-one-topology-tooanother"></a><span data-ttu-id="055a3-117">Hogyan Storm esemény hub spout ellenőrzőpont információinak átvitele egy topológia tooanother</span><span class="sxs-lookup"><span data-stu-id="055a3-117">How do I transfer Storm event hub spout checkpoint information from one topology tooanother</span></span>

<span data-ttu-id="055a3-118">Olvassa el az Azure Event Hubs hello segítségével a HDInsight alatt futó Storm event hub spout található .jar fájl topológiák fejlesztésekor a topológia, amelyen azonos nevet az új fürt hello kell telepítenie.</span><span class="sxs-lookup"><span data-stu-id="055a3-118">When you develop topologies that read from Azure Event Hubs by using hello HDInsight Storm event hub spout .jar file, you must deploy a topology that has hello same name on a new cluster.</span></span> <span data-ttu-id="055a3-119">Azonban meg kell őrizni hello ellenőrzőpont adat véglegesített tooApache ZooKeeper hello régi fürtön.</span><span class="sxs-lookup"><span data-stu-id="055a3-119">However, you must retain hello checkpoint data that was committed tooApache ZooKeeper on hello old cluster.</span></span>

### <a name="where-checkpoint-data-is-stored"></a><span data-ttu-id="055a3-120">Ellenőrzőpont-adatok tárolására</span><span class="sxs-lookup"><span data-stu-id="055a3-120">Where checkpoint data is stored</span></span>
<span data-ttu-id="055a3-121">Az eltolások ellenőrzőpont adatait hello event hub spout ZooKeeper a két legfelső szintű elérési útjában tárolja:</span><span class="sxs-lookup"><span data-stu-id="055a3-121">Checkpoint data for offsets is stored by hello event hub spout in ZooKeeper in two root paths:</span></span>
- <span data-ttu-id="055a3-122">Nem tranzakciós spout ellenőrzőpontokat /eventhubspout vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="055a3-122">Nontransactional spout checkpoints are stored in /eventhubspout.</span></span>
- <span data-ttu-id="055a3-123">Be- / tranzakciós tranzakciós spout ellenőrzőpont adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="055a3-123">Transactional spout checkpoint data is stored in /transactional.</span></span>

### <a name="how-toorestore"></a><span data-ttu-id="055a3-124">Hogyan toorestore</span><span class="sxs-lookup"><span data-stu-id="055a3-124">How toorestore</span></span>
<span data-ttu-id="055a3-125">tooget hello parancsfájlok és a szalagtárak ZooKeeper tooexport adatokat használni, és hello adatok hátsó tooZooKeeper egy új nevet, majd importálja [HDInsight alatt futó Storm példák](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span><span class="sxs-lookup"><span data-stu-id="055a3-125">tooget hello scripts and libraries that you use tooexport data out of ZooKeeper and then import hello data back tooZooKeeper with a new name, see [HDInsight Storm examples](https://github.com/hdinsight/hdinsight-storm-examples/tree/master/tools/zkdatatool-1.0).</span></span>

<span data-ttu-id="055a3-126">hello lib mappa hello megvalósítási hello exportálási/importálási művelethez tartalmazó .jar fájlokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="055a3-126">hello lib folder has .jar files that contain hello implementation for hello export/import operation.</span></span> <span data-ttu-id="055a3-127">hello bash mappa példa parancsfájl bemutatja, hogyan tooexport adatait hello ZooKeeper server hello régi fürt tartozik, és importálhatja vissza toohello ZooKeeper server hello új fürtön.</span><span class="sxs-lookup"><span data-stu-id="055a3-127">hello bash folder has an example script that demonstrates how tooexport data from hello ZooKeeper server on hello old cluster, and then import it back toohello ZooKeeper server on hello new cluster.</span></span>

<span data-ttu-id="055a3-128">Futtassa a hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) hello ZooKeeper csomópontok tooexport parancsfájl, és importálja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="055a3-128">Run hello [stormmeta.sh](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/tools/zkdatatool-1.0/bash/stormmeta.sh) script from hello ZooKeeper nodes tooexport and then import data.</span></span> <span data-ttu-id="055a3-129">Frissítés hello parancsfájl toohello megfelelő Hortonworks Data Platform (HDP) verziójával.</span><span class="sxs-lookup"><span data-stu-id="055a3-129">Update hello script toohello correct Hortonworks Data Platform (HDP) version.</span></span> <span data-ttu-id="055a3-130">(Jelenleg is dolgozunk a parancsfájlokat a Hdinsightban általános elvégzése.</span><span class="sxs-lookup"><span data-stu-id="055a3-130">(We are working on making these scripts generic in HDInsight.</span></span> <span data-ttu-id="055a3-131">Általános parancsfájlok futtathatók bármely olyan csomópontról hello fürt hello felhasználó módosítások nélkül.)</span><span class="sxs-lookup"><span data-stu-id="055a3-131">Generic scripts can run from any node on hello cluster without modifications by hello user.)</span></span>

<span data-ttu-id="055a3-132">hello exportálás parancs ír hello metaadatok tooan Apache Hadoop elosztott fájlrendszerrel (HDFS) útvonal (az Azure Blob Storage vagy az Azure Data Lake Store áruházban), amely egy helyen.</span><span class="sxs-lookup"><span data-stu-id="055a3-132">hello export command writes hello metadata tooan Apache Hadoop Distributed File System (HDFS) path (in an Azure Blob Storage or Azure Data Lake Store store) at a location that you set.</span></span>

### <a name="examples"></a><span data-ttu-id="055a3-133">Példák</span><span class="sxs-lookup"><span data-stu-id="055a3-133">Examples</span></span>

#### <a name="export-offset-metadata"></a><span data-ttu-id="055a3-134">Az eltolási metaadatok exportálása</span><span class="sxs-lookup"><span data-stu-id="055a3-134">Export offset metadata</span></span>
1. <span data-ttu-id="055a3-135">Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="055a3-135">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="055a3-136">Futtatási hello következő parancsot (frissítés után is használni hello HDP verzió-karakterlánca) tooexport ZooKeeper eltolás adatok toohello /stormmetadta/zkdata HDFS elérési útja:</span><span class="sxs-lookup"><span data-stu-id="055a3-136">Run hello following command (after you update hello HDP version string) tooexport ZooKeeper offset data toohello /stormmetadta/zkdata HDFS path:</span></span>

    ```apache   
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter export /eventhubspout /stormmetadata/zkdata
    ```

#### <a name="import-offset-metadata"></a><span data-ttu-id="055a3-137">Az eltolási metaadatok importálása</span><span class="sxs-lookup"><span data-stu-id="055a3-137">Import offset metadata</span></span>
1. <span data-ttu-id="055a3-138">Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="055a3-138">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="055a3-139">Futtatási hello (frissítés után is használni hello HDP verzió-karakterlánca) parancs következő tooimport ZooKeeper eltolás hello HDFS elérési útja/stormmetadata/zkdata toohello ZooKeeper server hello célfürtön adatait:</span><span class="sxs-lookup"><span data-stu-id="055a3-139">Run hello following command (after you update hello HDP version string) tooimport ZooKeeper offset data from hello HDFS path /stormmetadata/zkdata toohello ZooKeeper server on hello target cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter import /eventhubspout /home/sshadmin/zkdata
    ```
   
#### <a name="delete-offset-metadata-so-that-topologies-can-start-processing-data-from-hello-beginning-or-from-a-timestamp-that-hello-user-chooses"></a><span data-ttu-id="055a3-140">Így a topológiák adatfeldolgozás hello elejétől elindításához, vagy az időbélyeg hello felhasználó úgy dönt, eltolási metaadatok törlése</span><span class="sxs-lookup"><span data-stu-id="055a3-140">Delete offset metadata so that topologies can start processing data from hello beginning, or from a timestamp that hello user chooses</span></span>
1. <span data-ttu-id="055a3-141">Mely hello ellenőrzőponttól eltolás kell exportált toobe hello fürtön SSH toogo toohello ZooKeeper-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="055a3-141">Use SSH toogo toohello ZooKeeper cluster on hello cluster from which hello checkpoint offset needs toobe exported.</span></span>
2. <span data-ttu-id="055a3-142">Futtatási hello (frissítés után is használni hello HDP verzió-karakterlánca) parancs következő összes toodelete ZooKeeper eltolás hello aktuális fürt adatokat:</span><span class="sxs-lookup"><span data-stu-id="055a3-142">Run hello following command (after you update hello HDP version string) toodelete all ZooKeeper offset data in hello current cluster:</span></span>

    ```apache
    java -cp ./*:/etc/hadoop/conf/*:/usr/hdp/2.5.1.0-56/hadoop/*:/usr/hdp/2.5.1.0-56/hadoop/lib/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/*:/usr/hdp/2.5.1.0-56/hadoop-hdfs/lib/*:/etc/failover-controller/conf/*:/etc/hadoop/* com.microsoft.storm.zkdatatool.ZkdataImporter delete /eventhubspout
    ```

## <a name="how-do-i-locate-storm-binaries-on-a-cluster"></a><span data-ttu-id="055a3-143">Hogyan keresse meg a Storm bináris fürt</span><span class="sxs-lookup"><span data-stu-id="055a3-143">How do I locate Storm binaries on a cluster</span></span>
<span data-ttu-id="055a3-144">A Storm bináris hello aktuális HDP verem /usr/hdp/current/storm-client szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="055a3-144">Storm binaries for hello current HDP stack are in /usr/hdp/current/storm-client.</span></span> <span data-ttu-id="055a3-145">hello helyre van hello azonos átjárócsomópontokkal, valamint az ezen csomópontokhoz.</span><span class="sxs-lookup"><span data-stu-id="055a3-145">hello location is hello same both for head nodes and for worker nodes.</span></span>
 
<span data-ttu-id="055a3-146">Előfordulhat, hogy több bináris fájljait (például /usr/hdp/2.5.0.1233/storm) /usr/hdp adott HDP verzióihoz.</span><span class="sxs-lookup"><span data-stu-id="055a3-146">There might be multiple binaries for specific HDP versions in /usr/hdp (for example, /usr/hdp/2.5.0.1233/storm).</span></span> <span data-ttu-id="055a3-147">hello /usr/hdp/current/storm-client mappa symlinked toohello legújabb verziójára, amely hello fürtön fut.</span><span class="sxs-lookup"><span data-stu-id="055a3-147">hello /usr/hdp/current/storm-client folder is symlinked toohello latest version that is running on hello cluster.</span></span>

<span data-ttu-id="055a3-148">További információkért lásd: [Connect tooan HDInsight-fürthöz SSH segítségével](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) és [Storm](http://storm.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="055a3-148">For more information, see [Connect tooan HDInsight cluster by using SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-linux-use-ssh-unix) and [Storm](http://storm.apache.org/).</span></span>
 
## <a name="how-do-i-determine-hello-deployment-topology-of-a-storm-cluster"></a><span data-ttu-id="055a3-149">Hogyan állapítható meg hello telepítési topológia a Storm-fürt</span><span class="sxs-lookup"><span data-stu-id="055a3-149">How do I determine hello deployment topology of a Storm cluster</span></span>
<span data-ttu-id="055a3-150">Először azonosítsa a HDInsight alatt futó Storm, telepített összetevők.</span><span class="sxs-lookup"><span data-stu-id="055a3-150">First, identify all components that are installed with HDInsight Storm.</span></span> <span data-ttu-id="055a3-151">A Storm-fürt négy csomópont kategóriák áll:</span><span class="sxs-lookup"><span data-stu-id="055a3-151">A Storm cluster consists of four node categories:</span></span>

* <span data-ttu-id="055a3-152">Átjárócsomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-152">Gateway nodes</span></span>
* <span data-ttu-id="055a3-153">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-153">Head nodes</span></span>
* <span data-ttu-id="055a3-154">ZooKeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-154">ZooKeeper nodes</span></span>
* <span data-ttu-id="055a3-155">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="055a3-155">Worker nodes</span></span>
 
### <a name="gateway-nodes"></a><span data-ttu-id="055a3-156">Átjárócsomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-156">Gateway nodes</span></span>
<span data-ttu-id="055a3-157">Átjáró csomópont egy átjáró és a fordított proxy szolgáltatás, amely lehetővé teszi, hogy a nyilvános hozzáférés tooan active Ambari management szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="055a3-157">A gateway node is a gateway and reverse proxy service that enables public access tooan active Ambari management service.</span></span> <span data-ttu-id="055a3-158">Azt is kezeli az Ambari vezető választás.</span><span class="sxs-lookup"><span data-stu-id="055a3-158">It also handles Ambari leader election.</span></span>
 
### <a name="head-nodes"></a><span data-ttu-id="055a3-159">HEAD csomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-159">Head nodes</span></span>
<span data-ttu-id="055a3-160">A Storm átjárócsomópontokkal futtassa a következő szolgáltatások hello:</span><span class="sxs-lookup"><span data-stu-id="055a3-160">Storm head nodes run hello following services:</span></span>
* <span data-ttu-id="055a3-161">Nimbus</span><span class="sxs-lookup"><span data-stu-id="055a3-161">Nimbus</span></span>
* <span data-ttu-id="055a3-162">Ambari kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="055a3-162">Ambari server</span></span>
* <span data-ttu-id="055a3-163">Ambari metrikák kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="055a3-163">Ambari Metrics server</span></span>
* <span data-ttu-id="055a3-164">Ambari metrikákat gyűjtő</span><span class="sxs-lookup"><span data-stu-id="055a3-164">Ambari Metrics Collector</span></span>
 
### <a name="zookeeper-nodes"></a><span data-ttu-id="055a3-165">ZooKeeper csomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-165">ZooKeeper nodes</span></span>
<span data-ttu-id="055a3-166">HDInsight egy három csomópontos ZooKeeper kvórum tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="055a3-166">HDInsight comes with a three-node ZooKeeper quorum.</span></span> <span data-ttu-id="055a3-167">hello kvórum mérete rögzített, és nem kell újrakonfigurálni.</span><span class="sxs-lookup"><span data-stu-id="055a3-167">hello quorum size is fixed, and cannot be reconfigured.</span></span>
 
<span data-ttu-id="055a3-168">A Storm szolgáltatások hello fürtben konfigurált tooautomatically használata hello ZooKeeper kvórum.</span><span class="sxs-lookup"><span data-stu-id="055a3-168">Storm services in hello cluster are configured tooautomatically use hello ZooKeeper quorum.</span></span>
 
### <a name="worker-nodes"></a><span data-ttu-id="055a3-169">Munkavégző csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="055a3-169">Worker nodes</span></span>
<span data-ttu-id="055a3-170">Storm munkavégző csomópontokhoz futtassa a következő szolgáltatások hello:</span><span class="sxs-lookup"><span data-stu-id="055a3-170">Storm worker nodes run hello following services:</span></span>
* <span data-ttu-id="055a3-171">Felügyeleti</span><span class="sxs-lookup"><span data-stu-id="055a3-171">Supervisor</span></span>
* <span data-ttu-id="055a3-172">Munkavégző Java virtuális gépek (JVMs), a futó topológiák</span><span class="sxs-lookup"><span data-stu-id="055a3-172">Worker Java virtual machines (JVMs), for running topologies</span></span>
* <span data-ttu-id="055a3-173">Ambari ügynök</span><span class="sxs-lookup"><span data-stu-id="055a3-173">Ambari agent</span></span>
 
## <a name="how-do-i-locate-storm-event-hub-spout-binaries-for-development"></a><span data-ttu-id="055a3-174">Hogyan keresse meg a Storm event hub spout bináris fejlesztési</span><span class="sxs-lookup"><span data-stu-id="055a3-174">How do I locate Storm event hub spout binaries for development</span></span>
 
<span data-ttu-id="055a3-175">A topológia a Storm event hub spout .jar fájlok használatával kapcsolatos további információkért tekintse meg a következő erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="055a3-175">For more information about using Storm event hub spout .jar files with your topology, see hello following resources.</span></span>
 
### <a name="java-based-topology"></a><span data-ttu-id="055a3-176">Java-alapú topológia</span><span class="sxs-lookup"><span data-stu-id="055a3-176">Java-based topology</span></span>
[<span data-ttu-id="055a3-177">Az Azure Event Hubs (Java) futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="055a3-177">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-java-event-hub-topology)
 
### <a name="c-based-topology-mono-on-hdinsight-34-linux-storm-clusters"></a><span data-ttu-id="055a3-178">C#-alapú topológia (a HDInsight 3.4 + Linux Storm-fürtök monó)</span><span class="sxs-lookup"><span data-stu-id="055a3-178">C#-based topology (Mono on HDInsight 3.4+ Linux Storm clusters)</span></span>
[<span data-ttu-id="055a3-179">Az Azure Event Hubs-eseményközpontok eseményeinek feldolgozása a HDInsight alatt futó Stormmal (C#)</span><span class="sxs-lookup"><span data-stu-id="055a3-179">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-develop-csharp-event-hub-topology)
 
### <a name="latest-storm-event-hub-spout-binaries-for-hdinsight-35-linux-storm-clusters"></a><span data-ttu-id="055a3-180">Legújabb Storm event hub spout binárist 3.5 + HDInsight Linux Storm-fürtök</span><span class="sxs-lookup"><span data-stu-id="055a3-180">Latest Storm event hub spout binaries for HDInsight 3.5+ Linux Storm clusters</span></span>
<span data-ttu-id="055a3-181">toolearn hogyan toouse hello legújabb Storm event hub spout, amely kompatibilis a HDInsight 3.5 + Linux Storm-fürtök, tekintse meg a hello mvn-tárház [információs fájl](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span><span class="sxs-lookup"><span data-stu-id="055a3-181">toolearn how toouse hello latest Storm event hub spout that works with HDInsight 3.5+ Linux Storm clusters, see hello mvn-repo [readme file](https://github.com/hdinsight/mvn-repo/blob/master/README.md).</span></span>
 
### <a name="source-code-examples"></a><span data-ttu-id="055a3-182">Forrás-kódpéldák</span><span class="sxs-lookup"><span data-stu-id="055a3-182">Source code examples</span></span>
<span data-ttu-id="055a3-183">Lásd: [példák](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) , hogy hogyan tooread és az Azure Event Hubs egy Apache Storm-topológia (Java nyelven írt) használata az Azure HDInsight-fürtök írása.</span><span class="sxs-lookup"><span data-stu-id="055a3-183">See [examples](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) of how tooread and write from Azure Event Hub using an Apache Storm topology (written in Java) on an Azure HDInsight cluster.</span></span>
 
## <a name="how-do-i-locate-storm-log4j-configuration-files-on-clusters"></a><span data-ttu-id="055a3-184">Hogyan keresse meg a Storm Log4J konfigurációs fájlok fürtökön</span><span class="sxs-lookup"><span data-stu-id="055a3-184">How do I locate Storm Log4J configuration files on clusters</span></span>
 
<span data-ttu-id="055a3-185">a Storm szolgáltatások tooidentify Apache Log4J konfigurációs fájljait.</span><span class="sxs-lookup"><span data-stu-id="055a3-185">tooidentify Apache Log4J configuration files for Storm services.</span></span>
 
### <a name="on-head-nodes"></a><span data-ttu-id="055a3-186">Az átjárócsomópontokkal</span><span class="sxs-lookup"><span data-stu-id="055a3-186">On head nodes</span></span>
<span data-ttu-id="055a3-187">hello Nimbus Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="055a3-187">hello Nimbus Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
### <a name="on-worker-nodes"></a><span data-ttu-id="055a3-188">A feldolgozó csomópontok</span><span class="sxs-lookup"><span data-stu-id="055a3-188">On worker nodes</span></span>
<span data-ttu-id="055a3-189">hello felügyelő Log4J konfiguráció olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/cluster.xml.</span><span class="sxs-lookup"><span data-stu-id="055a3-189">hello supervisor Log4J configuration is read from /usr/hdp/\<HDP version\>/storm/log4j2/cluster.xml.</span></span>
 
<span data-ttu-id="055a3-190">hello munkavégző Log4J konfigurációs fájl olvasása az/usr/hdp/\<HDP verzió\>/storm/log4j2/worker.xml.</span><span class="sxs-lookup"><span data-stu-id="055a3-190">hello worker Log4J configuration file is read from /usr/hdp/\<HDP version\>/storm/log4j2/worker.xml.</span></span>
 
<span data-ttu-id="055a3-191">Példák: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span><span class="sxs-lookup"><span data-stu-id="055a3-191">Examples: /usr/hdp/2.6.0.2-76/storm/log4j2/cluster.xml /usr/hdp/2.6.0.2-76/storm/log4j2/worker.xml</span></span>

