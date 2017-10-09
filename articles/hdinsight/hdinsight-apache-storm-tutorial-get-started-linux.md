---
title: "aaaStorm-kezdőpéldák a HDInsight - Azure alatt futó Apache Storm |} Microsoft Docs"
description: "Megtudhatja, hogyan toodo big data-elemzések és a valós idejű adatfeldolgozásra Apache Storm használatának és a HDInsight alatt futó storm-starter-példák hello."
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
ms.openlocfilehash: bb6d6549e67ca5b557f0692f98c89692a87267b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
#<a name="get-started-with-apache-storm-on-hdinsight-using-hello-storm-starter-examples"></a><span data-ttu-id="851a8-104">Ismerkedés az Apache Storm on HDInsight hello storm-kezdőpéldák a</span><span class="sxs-lookup"><span data-stu-id="851a8-104">Get started with Apache Storm on HDInsight using hello storm-starter examples</span></span>

<span data-ttu-id="851a8-105">Ismerje meg, hogyan toouse alatt futó Apache Storm a Hdinsightban az hello storm-kezdőpéldák.</span><span class="sxs-lookup"><span data-stu-id="851a8-105">Learn how toouse Apache Storm in HDInsight using hello storm-starter examples.</span></span>

<span data-ttu-id="851a8-106">Az Apache Storm egy skálázható, hibatűrő, elosztott, valós idejű számítási rendszer az adatstreamek feldolgozására.</span><span class="sxs-lookup"><span data-stu-id="851a8-106">Apache Storm is a scalable, fault-tolerant, distributed, real-time computation system for processing streams of data.</span></span> <span data-ttu-id="851a8-107">A Storm on Azure HDInsight segítségével olyan felhőalapú Storm-fürtöket hozhat létre, amelyek valós időben végeznek big data elemzést.</span><span class="sxs-lookup"><span data-stu-id="851a8-107">With Storm on Azure HDInsight, you can create a cloud-based Storm cluster that performs big data analytics in real time.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="851a8-108">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="851a8-108">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="851a8-109">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="851a8-109">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="851a8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="851a8-110">Prerequisites</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

* <span data-ttu-id="851a8-111">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="851a8-111">**An Azure subscription**.</span></span> <span data-ttu-id="851a8-112">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="851a8-112">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="851a8-113">**SSH- és SCP-ismeretek**.</span><span class="sxs-lookup"><span data-stu-id="851a8-113">**Familiarity with SSH and SCP**.</span></span> <span data-ttu-id="851a8-114">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="851a8-114">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-a-storm-cluster"></a><span data-ttu-id="851a8-115">Storm-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="851a8-115">Create a Storm cluster</span></span>

<span data-ttu-id="851a8-116">Következő lépések toocreate Storm on HDInsight-fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="851a8-116">Use hello following steps toocreate a Storm on HDInsight cluster:</span></span>

1. <span data-ttu-id="851a8-117">A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új**, **Eszközintelligencia + analitika**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="851a8-117">From hello [Azure portal](https://portal.azure.com), select **+ NEW**, **Intelligence + Analytics**, and then select **HDInsight**.</span></span>

    ![HDInsight-fürt létrehozása](./media/hdinsight-apache-storm-tutorial-get-started-linux/create-hdinsight.png)

2. <span data-ttu-id="851a8-119">A hello **alapjai** panelen adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="851a8-119">From hello **Basics** blade, enter hello following information:</span></span>

    * <span data-ttu-id="851a8-120">**A fürt neve**: hello hello HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="851a8-120">**Cluster Name**: hello name of hello HDInsight cluster.</span></span>
    * <span data-ttu-id="851a8-121">**Előfizetés**: hello előfizetés toouse kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="851a8-121">**Subscription**: Select hello subscription toouse.</span></span>
    * <span data-ttu-id="851a8-122">**A fürt bejelentkezési felhasználónevének** és **fürt bejelentkezési jelszó**: hello bejelentkezési hello fürt eléréséhez a HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="851a8-122">**Cluster login username** and **Cluster login password**: hello login when accessing hello cluster over HTTPS.</span></span> <span data-ttu-id="851a8-123">A hitelesítő adatok tooaccess szolgáltatások például hello Ambari webes felhasználói felületén vagy a REST API-t használja.</span><span class="sxs-lookup"><span data-stu-id="851a8-123">You use these credentials tooaccess services such as hello Ambari Web UI or REST API.</span></span>
    * <span data-ttu-id="851a8-124">**Secure Shell (SSH) felhasználónév**: hello bejelentkezési SSH-n keresztül hello fürt eléréséhez használt.</span><span class="sxs-lookup"><span data-stu-id="851a8-124">**Secure Shell (SSH) username**: hello login used when accessing hello cluster over SSH.</span></span> <span data-ttu-id="851a8-125">Alapértelmezés szerint hello jelszó hello ugyanaz, mint a hello fürt bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="851a8-125">By default hello password is hello same as hello cluster login password.</span></span>
    * <span data-ttu-id="851a8-126">**Erőforráscsoport**: hello erőforrás csoport toocreate hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="851a8-126">**Resource Group**: hello resource group toocreate hello cluster in.</span></span>
    * <span data-ttu-id="851a8-127">**Hely**: hello Azure-régiót toocreate hello fürtöt.</span><span class="sxs-lookup"><span data-stu-id="851a8-127">**Location**: hello Azure region toocreate hello cluster in.</span></span>

    ![Előfizetés kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-basic-configuration.png)

3. <span data-ttu-id="851a8-129">Válassza ki **típusú fürt**, és a set hello majd értékek következő hello **fürtkonfiguráció** panel:</span><span class="sxs-lookup"><span data-stu-id="851a8-129">Select **Cluster type**, and then set hello following values on hello **Cluster configuration** blade:</span></span>

    * <span data-ttu-id="851a8-130">**Fürt típusa**: Storm</span><span class="sxs-lookup"><span data-stu-id="851a8-130">**Cluster Type**: Storm</span></span>

    * <span data-ttu-id="851a8-131">**Operációs rendszer**: Linux</span><span class="sxs-lookup"><span data-stu-id="851a8-131">**Operating system**: Linux</span></span>

    * <span data-ttu-id="851a8-132">**Verzió**: Storm 1.1.0 (HDI 3.6)</span><span class="sxs-lookup"><span data-stu-id="851a8-132">**Version**: Storm 1.1.0 (HDI 3.6)</span></span>

    * <span data-ttu-id="851a8-133">**Fürt szintje**: Standard</span><span class="sxs-lookup"><span data-stu-id="851a8-133">**Cluster Tier**: Standard</span></span>

    <span data-ttu-id="851a8-134">Végül, használja a hello **válasszon** toosave beállítások gombra.</span><span class="sxs-lookup"><span data-stu-id="851a8-134">Finally, use hello **Select** button toosave settings.</span></span>

    ![Fürttípus kiválasztása](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-cluster-type.png)

4. <span data-ttu-id="851a8-136">Hello fürt típusának kiválasztása után használja a hello __válasszon__ gomb tooset hello fürt típusa.</span><span class="sxs-lookup"><span data-stu-id="851a8-136">After selecting hello cluster type, use hello __Select__ button tooset hello cluster type.</span></span> <span data-ttu-id="851a8-137">Következő lépésként az hello __következő__ gomb toofinish alapszintű konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="851a8-137">Next, use hello __Next__ button toofinish basic configuration.</span></span>

5. <span data-ttu-id="851a8-138">A hello **tárolási** panelen válassza ki vagy hozzon létre egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="851a8-138">From hello **Storage** blade, select or create a Storage account.</span></span> <span data-ttu-id="851a8-139">A jelen dokumentumban leírt lépések hello, hello többi mezőt hagyja a panel hello alapértelmezett értéken.</span><span class="sxs-lookup"><span data-stu-id="851a8-139">For hello steps in this document, leave hello other fields on this blade at hello default values.</span></span> <span data-ttu-id="851a8-140">Használjon hello __következő__ gomb toosave tárolási konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="851a8-140">Use hello __Next__ button toosave storage configuration.</span></span>

    ![Állítsa be a HDInsight-hello tárolási fiók beállításai](./media/hdinsight-apache-storm-tutorial-get-started-linux/set-hdinsight-storage-account.png)

6. <span data-ttu-id="851a8-142">A hello **összegzés** panelen hello fürt hello konfiguráció áttekintése.</span><span class="sxs-lookup"><span data-stu-id="851a8-142">From hello **Summary** blade, review hello configuration for hello cluster.</span></span> <span data-ttu-id="851a8-143">Használjon hello __szerkesztése__ hivatkozásait toochange életbe léptetett beállítások helytelenek.</span><span class="sxs-lookup"><span data-stu-id="851a8-143">Use hello __Edit__ links toochange any settings that are incorrect.</span></span> <span data-ttu-id="851a8-144">Végezetül the__Create__ gomb toocreate hello-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="851a8-144">Finally, use the__Create__ button toocreate hello cluster.</span></span>

    ![A fürtkonfiguráció összegzése](./media/hdinsight-apache-storm-tutorial-get-started-linux/hdinsight-configuration-summary.png)

    > [!NOTE]
    > <span data-ttu-id="851a8-146">Too20 perc toocreate hello fürt is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="851a8-146">It can take up too20 minutes toocreate hello cluster.</span></span>

## <a name="run-a-storm-starter-sample-on-hdinsight"></a><span data-ttu-id="851a8-147">Storm Starter-példa futtatása a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="851a8-147">Run a storm-starter sample on HDInsight</span></span>

1. <span data-ttu-id="851a8-148">Csatlakozzon az SSH használatával toohello HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="851a8-148">Connect toohello HDInsight cluster using SSH:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="851a8-149">Ha a jelszó toosecure az SSH-felhasználói fiókhoz,-e rákérdezéses tooenter azt.</span><span class="sxs-lookup"><span data-stu-id="851a8-149">If you used a password toosecure your SSH user account, you are prompted tooenter it.</span></span> <span data-ttu-id="851a8-150">A nyilvános kulcs használatakor kell segítségével hello `-i` paraméter toospecify hello kapcsolódó titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="851a8-150">If you used a public key, you may need use hello `-i` parameter toospecify hello matching private key.</span></span> <span data-ttu-id="851a8-151">Például: `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="851a8-151">For example, `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`.</span></span>

    <span data-ttu-id="851a8-152">További információk: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="851a8-152">For information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="851a8-153">A következő parancs toostart példatopológia hello használata:</span><span class="sxs-lookup"><span data-stu-id="851a8-153">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology wordcount

    > [!NOTE]
    > <span data-ttu-id="851a8-154">A HDInsight korábbi verzióiban hello topológia hello osztály neve nem `storm.starter.WordCountTopology` helyett `org.apache.storm.starter.WordCountTopology`.</span><span class="sxs-lookup"><span data-stu-id="851a8-154">On previous versions of HDInsight, hello class name of hello topology is `storm.starter.WordCountTopology` instead of `org.apache.storm.starter.WordCountTopology`.</span></span>

    <span data-ttu-id="851a8-155">Indítja el a hello példa WordCount topológia hello fürtön, a "wordcount" rövid nevet.</span><span class="sxs-lookup"><span data-stu-id="851a8-155">This command starts hello example WordCount topology on hello cluster, with a friendly name of 'wordcount'.</span></span> <span data-ttu-id="851a8-156">Véletlenszerűen létrehozott mondatokat és count hello előfordulásainak kívánt számát, az összes szó hello mondat a.</span><span class="sxs-lookup"><span data-stu-id="851a8-156">It randomly generates sentences and count hello occurrence of each word in hello sentences.</span></span>

    > [!NOTE]
    > <span data-ttu-id="851a8-157">A saját topológiák toohello fürtre történő elküldésekor, át kell másolnia hello használata előtt hello fürtöt tartalmazó jar-fájlra hello `storm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="851a8-157">When submitting your own topologies toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="851a8-158">Használjon hello `scp` toocopy hello parancsfájlt.</span><span class="sxs-lookup"><span data-stu-id="851a8-158">Use hello `scp` command toocopy hello file.</span></span> <span data-ttu-id="851a8-159">Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="851a8-159">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
    >
    > <span data-ttu-id="851a8-160">hello WordCount-példa és egyéb storm-starter-példák megtalálhatóak a fürthöz `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="851a8-160">hello WordCount example, and other storm-starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

<span data-ttu-id="851a8-161">Ha érdekli a hello storm-kezdőpéldák hello forrás megtekintése, hello kódot található [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span><span class="sxs-lookup"><span data-stu-id="851a8-161">If you are interested in viewing hello source for hello storm-starter examples, you can find hello code at [https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter](https://github.com/apache/storm/tree/1.1.x-branch/examples/storm-starter).</span></span> <span data-ttu-id="851a8-162">A hivatkozás a Storm 1.1.x-es verzió példáira mutat, amely a HDInsight 3.6-ban érhető el.</span><span class="sxs-lookup"><span data-stu-id="851a8-162">This link is for Storm 1.1.x, which is provided with HDInsight 3.6.</span></span> <span data-ttu-id="851a8-163">A Storm egyéb verziói esetén használja a hello __fiókirodai__ hello lap tooselect hello tetején gombra egy másik Storm verzióját.</span><span class="sxs-lookup"><span data-stu-id="851a8-163">For other versions of Storm, use hello __Branch__ button at hello top of hello page tooselect a different Storm version.</span></span>

## <a name="monitor-hello-topology"></a><span data-ttu-id="851a8-164">A figyelő hello topológia</span><span class="sxs-lookup"><span data-stu-id="851a8-164">Monitor hello topology</span></span>

<span data-ttu-id="851a8-165">hello Storm felhasználói felülete webes felületet biztosít a futó topológiákkal dolgozik, és szerepel-e a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="851a8-165">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span>

<span data-ttu-id="851a8-166">A következő lépéseket toomonitor hello topológiájának megtekintését a Storm felhasználói felülete hello hello használata:</span><span class="sxs-lookup"><span data-stu-id="851a8-166">Use hello following steps toomonitor hello topology using hello Storm UI:</span></span>

1. <span data-ttu-id="851a8-167">toodisplay hello Storm felhasználói felületén, nyissa meg egy webes böngésző toohttps://CLUSTERNAME.azurehdinsight.net/stormui.</span><span class="sxs-lookup"><span data-stu-id="851a8-167">toodisplay hello Storm UI, open a web browser toohttps://CLUSTERNAME.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="851a8-168">Cserélje le **CLUSTERNAME** hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="851a8-168">Replace **CLUSTERNAME** with hello name of your cluster.</span></span>

    > [!NOTE]
    > <span data-ttu-id="851a8-169">Ha tooprovide kéri a felhasználónevet és jelszót, adja meg a hello Fürtrendszergazda (rendszergazda) és a használt jelszó létrehozása hello fürt.</span><span class="sxs-lookup"><span data-stu-id="851a8-169">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

2. <span data-ttu-id="851a8-170">A **topológia összegzése**, jelölje be hello **wordcount** hello bejegyzést **neve** oszlop.</span><span class="sxs-lookup"><span data-stu-id="851a8-170">Under **Topology summary**, select hello **wordcount** entry in hello **Name** column.</span></span> <span data-ttu-id="851a8-171">Hello topológiára vonatkozó információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="851a8-171">Information about hello topology is displayed.</span></span>

    ![A Storm irányítópultja a Storm Starter WordCount-topológiára vonatkozó információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/topology-summary.png)

    <span data-ttu-id="851a8-173">Ezen a lapon hello a következő információkat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="851a8-173">This page provides hello following information:</span></span>

    * <span data-ttu-id="851a8-174">**Topológiastatisztikák** – alapszintű információkat tartalmaz hello topológia teljesítményével kapcsolatban, időtartományokba.</span><span class="sxs-lookup"><span data-stu-id="851a8-174">**Topology stats** - Basic information on hello topology performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="851a8-175">Ha a egy adott időpont ablak módosítások hello időkerete hello lap más szakaszaiban talál.</span><span class="sxs-lookup"><span data-stu-id="851a8-175">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="851a8-176">**Spoutok** – alapszintű információkat tartalmaz a spoutokkal kapcsolatban, beleértve az egyes spoutok által visszaadott legutóbbi hibaüzenetet hello.</span><span class="sxs-lookup"><span data-stu-id="851a8-176">**Spouts** - Basic information about spouts, including hello last error returned by each spout.</span></span>

    * <span data-ttu-id="851a8-177">**Boltok** – Alapszintű információkat tartalmaz a boltokkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="851a8-177">**Bolts** - Basic information about bolts.</span></span>

    * <span data-ttu-id="851a8-178">**Topológiakonfiguráció** -részletes hello topológia konfigurációjával kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="851a8-178">**Topology configuration** - Detailed information about hello topology configuration.</span></span>

    <span data-ttu-id="851a8-179">Ezen a lapon is biztosít, amelyen átvihető hello topológia műveletek:</span><span class="sxs-lookup"><span data-stu-id="851a8-179">This page also provides actions that can be taken on hello topology:</span></span>

    * <span data-ttu-id="851a8-180">**Aktiválás** – Folytatja az inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="851a8-180">**Activate** - Resumes processing of a deactivated topology.</span></span>

    * <span data-ttu-id="851a8-181">**Inaktiválás** – Megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="851a8-181">**Deactivate** - Pauses a running topology.</span></span>

    * <span data-ttu-id="851a8-182">**Visszaegyensúlyozás** -hello topológia párhuzamosságát hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="851a8-182">**Rebalance** - Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="851a8-183">Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello.</span><span class="sxs-lookup"><span data-stu-id="851a8-183">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="851a8-184">Párhuzamossági toocompensate hello növekedése/csökkenése számaként hello fürtben található csomópontok újraelosztás módosítja.</span><span class="sxs-lookup"><span data-stu-id="851a8-184">Rebalancing adjusts parallelism toocompensate for hello increased/decreased number of nodes in hello cluster.</span></span> <span data-ttu-id="851a8-185">További információkért lásd: [ismertetése a Storm-topológia párhuzamosságát hello](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="851a8-185">For more information, see [Understanding hello parallelism of a Storm topology](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

    * <span data-ttu-id="851a8-186">**Kill** – a Storm-topológia leállítása után hello megadott időkorlát.</span><span class="sxs-lookup"><span data-stu-id="851a8-186">**Kill** - Terminates a Storm topology after hello specified timeout.</span></span>

3. <span data-ttu-id="851a8-187">Ezen a lapon válassza ki egy bejegyzést hello **Spoutok** vagy **boltok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="851a8-187">From this page, select an entry from hello **Spouts** or **Bolts** section.</span></span> <span data-ttu-id="851a8-188">Kiválasztott összetevő hello információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="851a8-188">Information about hello selected component is displayed.</span></span>

    ![A Storm irányítópultja a kiválasztott összetevőkkel kapcsolatos információkkal.](./media/hdinsight-apache-storm-tutorial-get-started-linux/component-summary.png)

    <span data-ttu-id="851a8-190">Ez a lap megjeleníti a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="851a8-190">This page displays hello following information:</span></span>

    * <span data-ttu-id="851a8-191">**Spout vagy Bolt statisztikák** – alapszintű információkat tartalmaz hello összetevők teljesítményével kapcsolatban, időtartományokba.</span><span class="sxs-lookup"><span data-stu-id="851a8-191">**Spout/Bolt stats** - Basic information on hello component performance, organized into time windows.</span></span>

        > [!NOTE]
        > <span data-ttu-id="851a8-192">Ha a egy adott időpont ablak módosítások hello időkerete hello lap más szakaszaiban talál.</span><span class="sxs-lookup"><span data-stu-id="851a8-192">Selecting a specific time window changes hello time window for information displayed in other sections of hello page.</span></span>

    * <span data-ttu-id="851a8-193">**Beviteli statisztikák** (csak boltok esetében) - információk hello bolt által feldolgozott adatokat előállító összetevőkről.</span><span class="sxs-lookup"><span data-stu-id="851a8-193">**Input stats** (bolt only) - Information on components that produce data consumed by hello bolt.</span></span>

    * <span data-ttu-id="851a8-194">**Kimeneti statisztikák** – Információkat tartalmaz a bolt által kibocsátott adatokról.</span><span class="sxs-lookup"><span data-stu-id="851a8-194">**Output stats** - Information on data emitted by this bolt.</span></span>

    * <span data-ttu-id="851a8-195">**Végrehajtók** – Információkat tartalmaz a jelen összetevő példányairól.</span><span class="sxs-lookup"><span data-stu-id="851a8-195">**Executors** - Information on instances of this component.</span></span>

    * <span data-ttu-id="851a8-196">**Hibák** – A jelen összetevő által visszaadott hibaüzenetek.</span><span class="sxs-lookup"><span data-stu-id="851a8-196">**Errors** - Errors produced by this component.</span></span>

4. <span data-ttu-id="851a8-197">Ha adott spout vagy bolt hello részleteit jeleníti meg, jelöljön ki egy bejegyzést a hello **Port** hello oszlopa **végrehajtója** hello összetevők adott példánya részletes tooview szakasz.</span><span class="sxs-lookup"><span data-stu-id="851a8-197">When viewing hello details of a spout or bolt, select an entry from hello **Port** column in hello **Executors** section tooview details for a specific instance of hello component.</span></span>

        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["with"]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: split default ["nature"]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [snow]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [snow, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [white]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [white, 747293]
        2015-01-27 14:18:02 b.s.d.executor [INFO] Processing received message source: split:21, stream: default, id: {}, [seven]
        2015-01-27 14:18:02 b.s.d.task [INFO] Emitting: count default [seven, 1493957]

    <span data-ttu-id="851a8-198">Ebben a példában a word hello **hét** 1493957 hányszor történt.</span><span class="sxs-lookup"><span data-stu-id="851a8-198">In this example, hello word **seven** has occurred 1493957 times.</span></span> <span data-ttu-id="851a8-199">A számláló értéke hányszor hello word történt. Ez a topológia indítása óta.</span><span class="sxs-lookup"><span data-stu-id="851a8-199">This count is how many times hello word has been encountered since this topology was started.</span></span>

## <a name="stop-hello-topology"></a><span data-ttu-id="851a8-200">Hello topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="851a8-200">Stop hello topology</span></span>

<span data-ttu-id="851a8-201">Térjen vissza a toohello **topológia összegzése** hello word-count topológiához lapján, majd válassza ki a hello **Kill** hello a gomb **topológia műveletek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="851a8-201">Return toohello **Topology summary** page for hello word-count topology, and then select hello **Kill** button from hello **Topology actions** section.</span></span> <span data-ttu-id="851a8-202">Amikor a rendszer kéri, adja meg 10 másodperc toowait hello hello topológia leállítása előtt.</span><span class="sxs-lookup"><span data-stu-id="851a8-202">When prompted, enter 10 for hello seconds toowait before stopping hello topology.</span></span> <span data-ttu-id="851a8-203">Hello időkorlát, után hello topológia nem jelenik meg az hello felkeresésekor **Storm felhasználói felülete** hello irányítópult szakasza.</span><span class="sxs-lookup"><span data-stu-id="851a8-203">After hello timeout period, hello topology no longer appears when you visit hello **Storm UI** section of hello dashboard.</span></span>

## <a name="delete-hello-cluster"></a><span data-ttu-id="851a8-204">Hello fürt törlése</span><span class="sxs-lookup"><span data-stu-id="851a8-204">Delete hello cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="851a8-205">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="851a8-205">If you run into an issue with creating HDInsight cluster, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <span data-ttu-id="851a8-206"><a id="next"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="851a8-206"><a id="next"></a>Next steps</span></span>

<span data-ttu-id="851a8-207">Az Apache Storm oktatóanyag megismerte hello használata a HDInsight alatt futó Storm alapjait.</span><span class="sxs-lookup"><span data-stu-id="851a8-207">In this Apache Storm tutorial, you learned hello basics of working with Storm on HDInsight.</span></span> <span data-ttu-id="851a8-208">A következő megtudhatja, hogyan túl[Maven használatával fejlesztése Java-alapú topológiák](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="851a8-208">Next, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="851a8-209">Ha már ismeri a Java-alapú topológiák fejlesztése és egy meglévő topológiát tooHDInsight toodeploy, lásd: [központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md).</span><span class="sxs-lookup"><span data-stu-id="851a8-209">If you're already familiar with developing Java-based topologies and want toodeploy an existing topology tooHDInsight, see [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md).</span></span>

<span data-ttu-id="851a8-210">A.NET-fejlesztők C#- vagy hibrid C#/Java-topológiákat hozhatnak létre a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="851a8-210">If you are a .NET developer, you can create C# or hybrid C#/Java topologies using Visual Studio.</span></span> <span data-ttu-id="851a8-211">További információk: [C#-topológiák fejlesztése HDInsight alatt futó Apache Stormra a Visual Studio Hadoop-eszközeinek használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="851a8-211">For more information, see [Develop C# topologies for Apache Storm on HDInsight using Hadoop tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="851a8-212">Példa topológiákat, amely a HDInsight alatt futó Storm használható tekintse meg a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="851a8-212">For example topologies that can be used with Storm on HDInsight, see hello following examples:</span></span>

* [<span data-ttu-id="851a8-213">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="851a8-213">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

[apachestorm]: https://storm.incubator.apache.org
[stormdocs]: http://storm.incubator.apache.org/documentation/Documentation.html
[stormstarter]: https://github.com/apache/storm/tree/master/examples/storm-starter
[stormjavadocs]: https://storm.incubator.apache.org/apidocs/
[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[preview-portal]: https://portal.azure.com/
