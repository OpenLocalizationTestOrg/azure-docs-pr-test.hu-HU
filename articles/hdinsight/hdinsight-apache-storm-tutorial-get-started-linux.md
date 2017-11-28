---
title: "Storm Starter-példák az Azure HDInsight-alapú Apache Storm rendszerben | Microsoft Docs"
description: "Ismerje meg az Apache Storm használatával történő valós idejű big data-elemzést és adatfeldolgozást, valamint a HDInsight-alapú Storm Starter-példákat."
keywords: "storm starter, apache storm-példa"
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: d710dcac-35d1-4c27-a8d6-acaf8146b485
ms.service: hdinsight
ms.devlang: java
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 83fc6db1ddb43eb87e7c58684505d7196c1e53d0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-the-storm-starter-examples"></a><span data-ttu-id="2e381-104">A HDInsight-alapú Apache Storm rendszer használatának első lépései Storm Starter-példákkal</span><span class="sxs-lookup"><span data-stu-id="2e381-104">Get started with Apache Storm on HDInsight using the storm-starter examples</span></span>

<span data-ttu-id="2e381-105">Ismerje meg a HDInsight-alapú Apache Storm használatát a Storm Starter-példák segítségével.</span><span class="sxs-lookup"><span data-stu-id="2e381-105">Learn how to use Apache Storm in HDInsight using the storm-starter examples.</span></span>

<span data-ttu-id="2e381-106">Az Apache Storm egy skálázható, hibatűrő, elosztott, valós idejű számítási rendszer az adatstreamek feldolgozására.</span><span class="sxs-lookup"><span data-stu-id="2e381-106">Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data.</span></span> <span data-ttu-id="2e381-107">A Storm on Azure HDInsight segítségével olyan felhőalapú Storm-fürtöket hozhat létre, amelyek valós időben végeznek big data elemzést.</span><span class="sxs-lookup"><span data-stu-id="2e381-107">With Storm on Azure HDInsight, you can create a cloud-based Storm cluster that performs big data analytics in real time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2e381-108">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="2e381-108">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="2e381-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="2e381-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e381-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e381-110">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="2e381-111">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="2e381-111">**An Azure subscription**.</span></span> <span data-ttu-id="2e381-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="2e381-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="2e381-113">**SSH- és SCP-ismeretek**.</span><span class="sxs-lookup"><span data-stu-id="2e381-113">**Familiarity with SSH and SCP**.</span></span> <span data-ttu-id="2e381-114">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2e381-114">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-a-storm-cluster"></a><span data-ttu-id="2e381-115">Storm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e381-115">Create a Storm cluster</span></span>

<span data-ttu-id="2e381-116">HDInsight alatt futó Storm-fürt létrehozásához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2e381-116">Use the following steps to create a Storm on HDInsight cluster:</span></span>

1. <span data-ttu-id="2e381-117">Az [Azure Portalon](https://portal.azure.com) válassza az **+ ÚJ**, **Intelligencia és analitika**, majd a **HDInsight** elemet.</span><span class="sxs-lookup"><span data-stu-id="2e381-117">From the [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>

    ![HDInsight-fürt létrehozása](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. <span data-ttu-id="2e381-119">Az **Alapvető beállítások** panelen adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="2e381-119">From the **Basics** blade, enter the following information:</span></span>

    * <span data-ttu-id="2e381-120">**Fürt neve**: A HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="2e381-120">**Cluster Name**: The name of the HDInsight cluster.</span></span>
    * <span data-ttu-id="2e381-121">**Előfizetés**: Válassza ki a használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2e381-121">**Subscription**: Select the subscription to use.</span></span>
    * <span data-ttu-id="2e381-122">**Fürt bejelentkezési felhasználóneve** és **Fürt bejelentkezési jelszava**: A fürt HTTPS-kapcsolaton keresztüli elérésekor használt bejelentkezési adatok.</span><span class="sxs-lookup"><span data-stu-id="2e381-122">**Cluster login username** and **Cluster login password**: The login when accessing the cluster over HTTPS.</span></span> <span data-ttu-id="2e381-123">Ezekkel a hitelesítő adatokkal érheti el az olyan szolgáltatásokat, mint az Ambari webes felület vagy a REST API.</span><span class="sxs-lookup"><span data-stu-id="2e381-123">You use these credentials to access services such as the Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="2e381-124">**SSH-felhasználónév**: A fürt SSH-kapcsolaton keresztüli elérésekor használt bejelentkezési adatok.</span><span class="sxs-lookup"><span data-stu-id="2e381-124">**Secure Shell (SSH) username**: The login used when accessing the cluster over SSH.</span></span> <span data-ttu-id="2e381-125">Alapértelmezés szerint a jelszó megegyezik a fürt bejelentkezési jelszavával.</span><span class="sxs-lookup"><span data-stu-id="2e381-125">By default the password is the same as the cluster login password.</span></span>
    * <span data-ttu-id="2e381-126">**Erőforráscsoport**: Az az erőforráscsoport, amelyben a fürt létre lesz hozva.</span><span class="sxs-lookup"><span data-stu-id="2e381-126">**Resource Group**: The resource group to create the cluster in.</span></span>
    * <span data-ttu-id="2e381-127">**Hely**: Az az Azure-régió, amelyben a fürt létre lesz hozva.</span><span class="sxs-lookup"><span data-stu-id="2e381-127">**Location**: The Azure region to create the cluster in.</span></span>

    ![Előfizetés kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. <span data-ttu-id="2e381-129">Válassza ki a **Fürt típusát**, majd állítsa be a következő értékeket a **Fürtkonfiguráció** panelen:</span><span class="sxs-lookup"><span data-stu-id="2e381-129">Select **Cluster type**, and then set the following values on the **Cluster configuration** blade:</span></span>

    * <span data-ttu-id="2e381-130">**Fürt típusa**: Storm</span><span class="sxs-lookup"><span data-stu-id="2e381-130">**Cluster Type**: Storm</span></span>

    * <span data-ttu-id="2e381-131">**Operációs rendszer**: Linux</span><span class="sxs-lookup"><span data-stu-id="2e381-131">**Operating system**: Linux</span></span>

    * <span data-ttu-id="2e381-132">**Verzió**: Storm 1.1.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="2e381-132">**Version**: Storm 1.1.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="2e381-133">**Fürt szintje**: Standard</span><span class="sxs-lookup"><span data-stu-id="2e381-133">**Cluster Tier**: Standard</span></span>

    <span data-ttu-id="2e381-134">Végül mentse a beállításokat a **Kiválasztás** gomb használatával.</span><span class="sxs-lookup"><span data-stu-id="2e381-134">Finally, use the **Select** button to save settings.</span></span>

    ![Fürttípus kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="2e381-136">A fürt típusának kijelölése után erősítse meg a beállítást a __Kiválasztás__ gombbal.</span><span class="sxs-lookup"><span data-stu-id="2e381-136">After selecting the cluster type, use the __Select__ button to set the cluster type.</span></span> <span data-ttu-id="2e381-137">Ezután kattintson a __Tovább__ gombra az alapszintű konfiguráció befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2e381-137">Next, use the __Next__ button to finish basic configuration.</span></span>

5. <span data-ttu-id="2e381-138">A **Tárolás** panelen válasszon ki vagy hozzon létre egy Storage-fiókot.</span><span class="sxs-lookup"><span data-stu-id="2e381-138">From the **Storage** blade, select or create a Storage account.</span></span> <span data-ttu-id="2e381-139">A jelen dokumentumban leírt lépések esetében a panel többi mezőjét hagyja az alapértelmezett értékeken.</span><span class="sxs-lookup"><span data-stu-id="2e381-139">For the steps in this document, leave the other fields on this blade at the default values.</span></span> <span data-ttu-id="2e381-140">Kattintson a __Tovább__ gombra a tárolókonfiguráció mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="2e381-140">Use the __Next__ button to save storage configuration.</span></span>

    ![A tárfiók HDInsight-beállításainak konfigurálása](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. <span data-ttu-id="2e381-142">Az **Összegzés** lapon tekintse át a fürt konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="2e381-142">From the **Summary** blade, review the configuration for the cluster.</span></span> <span data-ttu-id="2e381-143">A __Szerkesztés__ hivatkozásai használatával módosítsa a hibás beállításokat.</span><span class="sxs-lookup"><span data-stu-id="2e381-143">Use the __Edit__ links to change any settings that are incorrect.</span></span> <span data-ttu-id="2e381-144">Végül kattintson a__Létrehozás__ gombra a fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="2e381-144">Finally, use the__Create__ button to create the cluster.</span></span>

    ![A fürtkonfiguráció összegzése](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > <span data-ttu-id="2e381-146">A fürt létrehozása 20 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="2e381-146">It can take up to 20 minutes to create the cluster.</span></span>

## <a name="run-a-storm-starter-sample-on-hdinsight"></a><span data-ttu-id="2e381-147">Storm Starter-példa futtatása a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="2e381-147">Run a storm-starter sample on HDInsight</span></span>

1. <span data-ttu-id="2e381-148">Csatlakozzon SSH-val a HDInsight-fürthöz:</span><span class="sxs-lookup"><span data-stu-id="2e381-148">Connect to the HDInsight cluster using SSH:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="2e381-149">Ha az SSH-felhasználói fiókhoz jelszót használt, a rendszer felkéri annak megadására.</span><span class="sxs-lookup"><span data-stu-id="2e381-149">If you used a password to secure your SSH user account, you are prompted to enter it.</span></span> <span data-ttu-id="2e381-150">Nyilvános kulcs használatakor lehetséges, hogy az `-i` paraméter használatára van szükség a megfelelő titkos kulcs megadásához.</span><span class="sxs-lookup"><span data-stu-id="2e381-150">If you used a public key, you may need use the `-i` parameter to specify the matching private key.</span></span> <span data-ttu-id="2e381-151">Például: `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="2e381-151">For example, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

    <span data-ttu-id="2e381-152">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="2e381-152">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="2e381-153">Használja az alábbi parancsot példatopológia indításához:</span><span class="sxs-lookup"><span data-stu-id="2e381-153">Use the following command to start an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > <span data-ttu-id="2e381-154">A HDInsight előző verzióiban a topológia osztályneve `org.apache.storm.starter.WordCountTopology` helyett `storm.starter.WordCountTopology`.</span><span class="sxs-lookup"><span data-stu-id="2e381-154">On previous versions of HDInsight, the class name of the topology is `storm.starter.WordCountTopology` instead of `org.apache.storm.starter.WordCountTopology`.</span></span>

    <span data-ttu-id="2e381-155">Ez a parancs a „wordcount” rövid néven elindítja a fürtön a WordCount-példatopológiát.</span><span class="sxs-lookup"><span data-stu-id="2e381-155">This command starts the example WordCount topology on the cluster, with a friendly name of 'wordcount'.</span></span> <span data-ttu-id="2e381-156">Ez véletlenszerűen állít elő mondatokat, majd az egyes szavak előfordulását számolja meg a mondatokban.</span><span class="sxs-lookup"><span data-stu-id="2e381-156">It randomly generates sentences and count the occurrence of each word in the sentences.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e381-157">A saját topológiák a fürtre történő elküldésekor a fürtöket tartalmazó jar-fájlt a `storm` parancs használata előtt kell másolnia.</span><span class="sxs-lookup"><span data-stu-id="2e381-157">When submitting your own topologies to the cluster, you must first copy the jar file containing the cluster before using the `storm` command.</span></span> <span data-ttu-id="2e381-158">A fájl másolásához használja az `scp` parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e381-158">Use the `scp` command to copy the file.</span></span> <span data-ttu-id="2e381-159">Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="2e381-159">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
    >
    > <span data-ttu-id="2e381-160">A WordCount-példa és más Storm Starter-példák már megtalálhatók a fürtön a következő helyen: `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="2e381-160">The WordCount example, and other storm-starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

<span data-ttu-id="2e381-161">A Storm Starter-példák forráskódját a [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter) helyen tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="2e381-161">If you are interested in viewing the source for the storm-starter examples, you can find the code at [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span></span> <span data-ttu-id="2e381-162">A hivatkozás a Storm 1.1.x-es verzió példáira mutat, amely a HDInsight 3.6-ban érhető el.</span><span class="sxs-lookup"><span data-stu-id="2e381-162">This link is for Storm 1.1.x, which is provided with HDInsight 3.6.</span></span> <span data-ttu-id="2e381-163">A Storm egyéb verziói esetében használja a __Branch__ (Ág) gombot a lap tetején egy másik Storm-verzió kiválasztásához.</span><span class="sxs-lookup"><span data-stu-id="2e381-163">For other versions of Storm, use the __Branch__ button at the top of the page to select a different Storm version.</span></span>

## <a name="monitor-the-topology"></a><span data-ttu-id="2e381-164">A topológia figyelése</span><span class="sxs-lookup"><span data-stu-id="2e381-164">Monitor the topology</span></span>

<span data-ttu-id="2e381-165">A HDInsight-fürtön elérhető Storm webes felhasználói felületet biztosít a futó topológiákkal való munkavégzéshez.</span><span class="sxs-lookup"><span data-stu-id="2e381-165">The Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span>

<span data-ttu-id="2e381-166">Kövesse az alábbi lépéseket a topológia a Storm felhasználói felületével történő figyeléséhez:</span><span class="sxs-lookup"><span data-stu-id="2e381-166">Use the following steps to monitor the topology using the Storm UI:</span></span>

1. <span data-ttu-id="2e381-167">A Storm felhasználói felületének megjelenítéséhez egy webböngészőben nyissa meg a következőt: https://CLUSTERNAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="2e381-167">To display the Storm UI, open a web browser to https://CLUSTERNAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="2e381-168">Cserélje le a **CLUSTERNAME** elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="2e381-168">Replace **CLUSTERNAME** with the name of your cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="2e381-169">Ha a rendszer felkéri a felhasználónév és a jelszó megadására, a fürt létrehozásakor használt fürtrendszergazda (rendszergazda) nevét és jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="2e381-169">If asked to provide a user name and password, enter the cluster administrator (admin) and password that you used when creating the cluster.</span></span>

2. <span data-ttu-id="2e381-170">A **Topology summary** (Topológia összegzése) területen válassza ki a **wordcount** bejegyzést a **Name** (Név) oszlopban.</span><span class="sxs-lookup"><span data-stu-id="2e381-170">Under **Topology summary**, select the **wordcount** entry in the **Name** column.</span></span> <span data-ttu-id="2e381-171">Megjelennek a topológiával kapcsolatos információk.</span><span class="sxs-lookup"><span data-stu-id="2e381-171">Information about the topology is displayed.</span></span>

    ![A Storm irányítópultja a Storm Starter WordCount-topológiára vonatkozó információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    <span data-ttu-id="2e381-173">Ezen a lapon az alábbi információk találhatóak meg:</span><span class="sxs-lookup"><span data-stu-id="2e381-173">This page provides the following information:</span></span>

    * <span data-ttu-id="2e381-174">**Topológiastatisztikák** – Alapszintű információkat tartalmaz a topológia teljesítményével kapcsolatban, időtartományokba rendezve.</span><span class="sxs-lookup"><span data-stu-id="2e381-174">**Topology stats** - Basic information on the topology performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2e381-175">Egy adott időtartományt kiválasztva a lap más szakaszaiban található információk időtartománya megváltozik.</span><span class="sxs-lookup"><span data-stu-id="2e381-175">Selecting a specific time window changes the time window for information displayed in other sections of the page.</span></span>

    * <span data-ttu-id="2e381-176">**Spoutok** – Alapszintű információkat tartalmaz a spoutokkal kapcsolatban, beleértve az egyes spoutok által visszaadott legutóbbi hibaüzenetet is.</span><span class="sxs-lookup"><span data-stu-id="2e381-176">**Spouts** - Basic information about spouts, including the last error returned by each spout.</span></span>

    * <span data-ttu-id="2e381-177">**Boltok** – Alapszintű információkat tartalmaz a boltokkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2e381-177">**Bolts** - Basic information about bolts.</span></span>

    * <span data-ttu-id="2e381-178">**Topológiakonfiguráció** – Részletes információkat tartalmaz a topológia konfigurációjával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2e381-178">**Topology configuration** - Detailed information about the topology configuration.</span></span>

    <span data-ttu-id="2e381-179">Ezen a lapon a következő topológiaműveletek szerepelnek:</span><span class="sxs-lookup"><span data-stu-id="2e381-179">This page also provides actions that can be taken on the topology:</span></span>

    * <span data-ttu-id="2e381-180">**Aktiválás** – Folytatja az inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="2e381-180">**Activate** - Resumes processing of a deactivated topology.</span></span>

    * <span data-ttu-id="2e381-181">**Inaktiválás** – Megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="2e381-181">**Deactivate** - Pauses a running topology.</span></span>

    * <span data-ttu-id="2e381-182">**Visszaegyensúlyozás** – Beállítja a topológia párhuzamosságát.</span><span class="sxs-lookup"><span data-stu-id="2e381-182">**Rebalance** - Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="2e381-183">A fürtben található csomópontok számának megváltoztatását követően újra ki kell egyensúlyozni a futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="2e381-183">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="2e381-184">Az újraegyensúlyozás beállítja a párhuzamosságot a fürtben található csomópontok számának növekedése/csökkenése kiegyensúlyozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="2e381-184">Rebalancing adjusts parallelism to compensate for the increased/decreased number of nodes in the cluster.</span></span> <span data-ttu-id="2e381-185">További információ: [Understanding the parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) (A Storm-topológia párhuzamosságának ismertetése).</span><span class="sxs-lookup"><span data-stu-id="2e381-185">For more information, see [Understanding the parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

    * <span data-ttu-id="2e381-186">**Törlés** – A Storm-topológia leállítása bizonyos időtúllépést követően.</span><span class="sxs-lookup"><span data-stu-id="2e381-186">**Kill** - Terminates a Storm topology after the specified timeout.</span></span>

3. <span data-ttu-id="2e381-187">Válassza ki a jelen lapon található **Spoutok** vagy **Boltok** szakaszok bejegyzéseinek egyikét.</span><span class="sxs-lookup"><span data-stu-id="2e381-187">From this page, select an entry from the **Spouts** or **Bolts** section.</span></span> <span data-ttu-id="2e381-188">Ekkor információk jelennek meg a kiválasztott összetevővel kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="2e381-188">Information about the selected component is displayed.</span></span>

    ![A Storm irányítópultja a kiválasztott összetevőkkel kapcsolatos információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    <span data-ttu-id="2e381-190">Ezen a lapon az alábbi információk találhatóak meg:</span><span class="sxs-lookup"><span data-stu-id="2e381-190">This page displays the following information:</span></span>

    * <span data-ttu-id="2e381-191">**Spout-/Bolt-statisztikák** – Alapszintű információkat tartalmaz az egyes összetevők teljesítményével kapcsolatban, időtartományokba rendezve.</span><span class="sxs-lookup"><span data-stu-id="2e381-191">**Spout/Bolt stats** - Basic information on the component performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="2e381-192">Egy adott időtartományt kiválasztva a lap más szakaszaiban található információk időtartománya megváltozik.</span><span class="sxs-lookup"><span data-stu-id="2e381-192">Selecting a specific time window changes the time window for information displayed in other sections of the page.</span></span>

    * <span data-ttu-id="2e381-193">**Beviteli statisztikák** (csak boltok esetében) – Információkat tartalmaz a bolt által feldolgozott adatokat előállító összetevőkről.</span><span class="sxs-lookup"><span data-stu-id="2e381-193">**Input stats** (bolt only) - Information on components that produce data consumed by the bolt.</span></span>

    * <span data-ttu-id="2e381-194">**Kimeneti statisztikák** – Információkat tartalmaz a bolt által kibocsátott adatokról.</span><span class="sxs-lookup"><span data-stu-id="2e381-194">**Output stats** - Information on data emitted by this bolt.</span></span>

    * <span data-ttu-id="2e381-195">**Végrehajtók** – Információkat tartalmaz a jelen összetevő példányairól.</span><span class="sxs-lookup"><span data-stu-id="2e381-195">**Executors** - Information on instances of this component.</span></span>

    * <span data-ttu-id="2e381-196">**Hibák** – A jelen összetevő által visszaadott hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="2e381-196">**Errors** - Errors produced by this component.</span></span>

4. <span data-ttu-id="2e381-197">Egy adott spout vagy bolt részletes adatainak megtekintésekor az összetevők adott példánya részletes adatainak megtekintéséhez jelöljön ki egy bejegyzést a **Port** oszlopból az **Executors** (Végrehajtók) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2e381-197">When viewing the details of a spout or bolt, select an entry from the **Port** column in the **Executors** section to view details for a specific instance of the component.</span></span>

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    <span data-ttu-id="2e381-198">Ebben a példában a **seven** szó 1 493 957 alkalommal fordul elő.</span><span class="sxs-lookup"><span data-stu-id="2e381-198">In this example, the word **seven** has occurred 1493957 times.</span></span> <span data-ttu-id="2e381-199">A szó előfordulásainak számlálása a topológia indításától kezdve történik.</span><span class="sxs-lookup"><span data-stu-id="2e381-199">This count is how many times the word has been encountered since this topology was started.</span></span>

## <a name="stop-the-topology"></a><span data-ttu-id="2e381-200">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="2e381-200">Stop the topology</span></span>

<span data-ttu-id="2e381-201">Lépjen vissza a **Topology summary** (Topológia összegzése) lapra a word-count topológiához, majd válassza a **Kill** (Törlés) gombot a **Topology actions** (Topológiaműveletek) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="2e381-201">Return to the **Topology summary** page for the word-count topology, and then select the **Kill** button from the **Topology actions** section.</span></span> <span data-ttu-id="2e381-202">Amikor a rendszer kéri, adjon meg 10 másodperces értéket a topológia leállítása előtti várakozási időként.</span><span class="sxs-lookup"><span data-stu-id="2e381-202">When prompted, enter 10 for the seconds to wait before stopping the topology.</span></span> <span data-ttu-id="2e381-203">Az időtúllépés lejáratát követően az adott topológia már nem jelenik meg az irányítópult **Storm felhasználói felülete** szakaszában.</span><span class="sxs-lookup"><span data-stu-id="2e381-203">After the timeout period, the topology no longer appears when you visit the **Storm UI** section of the dashboard.</span></span>

## <a name="delete-the-cluster"></a><span data-ttu-id="2e381-204">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="2e381-204">Delete the cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="2e381-205">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="2e381-205">If you run into an issue with creating HDInsight cluster, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <span data-ttu-id="2e381-206"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e381-206"><a id="next"></a>Next steps</span></span>

<span data-ttu-id="2e381-207">Ebből az Apache Storm-oktatóanyagból megismerhette a Storm HDInsightban való használatának alapjait.</span><span class="sxs-lookup"><span data-stu-id="2e381-207">In this Apache Storm tutorial, you learned the basics of working with Storm on HDInsight.</span></span> <span data-ttu-id="2e381-208">A következő szakaszban a [Java-alapú topológiák fejlesztését ismertetjük Maven használatával](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="2e381-208">Next, learn how to [Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="2e381-209">Ha már ismeri a Java-alapú topológiák fejlesztését, és egy meglévő topológiát kíván üzembe helyezni a HDInsighton, tekintse meg a következő cikket: [HDInsighton futó Apache Storm-topológiák üzembe helyezése és kezelése](hdinsight-storm-deploy-monitor-topology-linux.md).</span><span class="sxs-lookup"><span data-stu-id="2e381-209">If you're already familiar with developing Java-based topologies and want to deploy an existing topology to HDInsight, see [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span></span>

<span data-ttu-id="2e381-210">A.NET-fejlesztők C#- vagy hibrid C#/Java-topológiákat hozhatnak létre a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="2e381-210">If you are a .NET developer, you can create C# or hybrid C#/Java topologies using Visual Studio.</span></span> <span data-ttu-id="2e381-211">További információk: [C#-topológiák fejlesztése HDInsight alatt futó Apache Stormra a Visual Studio Hadoop-eszközeinek használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="2e381-211">For more information, see [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="2e381-212">A HDInsight alatt futó Stormmal használható példatopológiákat az alábbiakban talál:</span><span class="sxs-lookup"><span data-stu-id="2e381-212">For example topologies that can be used with Storm on HDInsight, see the following examples:</span></span>

* [<span data-ttu-id="2e381-213">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="2e381-213">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
