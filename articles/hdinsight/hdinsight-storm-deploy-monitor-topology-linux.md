---
title: "aaaDeploy és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy, figyelheti és kezelheti hello Storm irányítópultjának használata a Linux-alapú HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 35086e62-d6d8-4ccf-8cae-00073464a1e1
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 3a1edb773089cc596fea423710aa88cf83c7b841
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="73f79-104">Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="73f79-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="73f79-105">Ebből a dokumentumból megtudhatja, kezelése és figyelése a Storm a HDInsight-fürtökön futó Storm-topológiák hello alapjait.</span><span class="sxs-lookup"><span data-stu-id="73f79-105">In this document, learn hello basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73f79-106">hello cikkben leírt lépéseket igényelnek a Linux-alapú Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="73f79-106">hello steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="73f79-107">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="73f79-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="73f79-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="73f79-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="73f79-109">A telepítését és megfigyelését futó Windows-alapú HDInsight-topológiák további információkért lásd: [központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="73f79-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="73f79-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="73f79-110">Prerequisites</span></span>

* <span data-ttu-id="73f79-111">**A Linux-alapú Storm on HDInsight-fürt**: lásd: [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started-linux.md) fürt létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="73f79-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="73f79-112">(Választható) **SSH és az SCP ismeretét**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="73f79-113">(Választható) **Visual Studio**: Azure SDK 2.5.1-es vagy újabb és hello Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73f79-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and hello Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="73f79-114">További információkért lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="73f79-115">A következő Visual Studio verziójának hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="73f79-115">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="73f79-116">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="73f79-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="73f79-117">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="73f79-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="73f79-118">A Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="73f79-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="73f79-119">A Visual Studio 2015 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="73f79-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="73f79-120">A Visual Studio 2017 (minden kiadás).</span><span class="sxs-lookup"><span data-stu-id="73f79-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="73f79-121">A Data Lake Tools for Visual Studio 2017 hello Azure munkaterhelés részeként telepített.</span><span class="sxs-lookup"><span data-stu-id="73f79-121">Data Lake Tools for Visual Studio 2017 are installed as part of hello Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="73f79-122">Küldje el a topológia: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73f79-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="73f79-123">a HDInsight Tools hello lehet használt toosubmit C# vagy hibrid topológiák tooyour Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="73f79-123">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="73f79-124">a lépéseket követve hello mintaalkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="73f79-124">hello following steps use a sample application.</span></span> <span data-ttu-id="73f79-125">A saját topológiák hello a HDInsight Tools használatával létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-125">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="73f79-126">Ha még nem telepítette hello legújabb verziójának hello Data Lake tools for Visual Studio, lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-126">If you have not already installed hello latest version of hello Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="73f79-127">a Data Lake Tools for Visual Studio hello volt korábbi nevén hello a HDInsight Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="73f79-127">hello Data Lake Tools for Visual Studio were formerly called hello HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="73f79-128">A Data Lake Tools for Visual Studio szerepelnek hello __Azure munkaterhelés__ a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="73f79-128">Data Lake Tools for Visual Studio are included in hello __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="73f79-129">Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="73f79-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="73f79-130">A hello **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="73f79-130">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="73f79-131">Válassza ki a sablonok hello listáról **Storm minta**.</span><span class="sxs-lookup"><span data-stu-id="73f79-131">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="73f79-132">Hello hello párbeszédpanel alsó részén írja be a hello alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="73f79-132">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="73f79-134">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="73f79-134">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="73f79-135">Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="73f79-135">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="73f79-136">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.</span><span class="sxs-lookup"><span data-stu-id="73f79-136">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="73f79-137">Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="73f79-137">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="73f79-138">Figyelheti, hogy hello elküldése sikeres hello segítségével **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="73f79-138">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-hello-storm-command"></a><span data-ttu-id="73f79-139">Küldje el a topológia: SSH és hello Storm parancs</span><span class="sxs-lookup"><span data-stu-id="73f79-139">Submit a topology: SSH and hello Storm command</span></span>

1. <span data-ttu-id="73f79-140">SSH tooconnect toohello HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="73f79-140">Use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="73f79-141">Cserélje le **felhasználónév** az SSH-bejelentkezéskor hello nevét.</span><span class="sxs-lookup"><span data-stu-id="73f79-141">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="73f79-142">Cserélje le **CLUSTERNAME** a HDInsight fürt nevű:</span><span class="sxs-lookup"><span data-stu-id="73f79-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="73f79-143">További információ az SSH tooconnect tooyour HDInsight használatával fürt című [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-143">For more information on using SSH tooconnect tooyour HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="73f79-144">A következő parancs toostart példatopológia hello használata:</span><span class="sxs-lookup"><span data-stu-id="73f79-144">Use hello following command toostart an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="73f79-145">Indítja el a hello példa WordCount topológia hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="73f79-145">This command starts hello example WordCount topology on hello cluster.</span></span> <span data-ttu-id="73f79-146">Ez a topológia véletlenszerű előállításához mondatokat és count hello előfordulásainak kívánt számát, az összes szó a hello mondatokat.</span><span class="sxs-lookup"><span data-stu-id="73f79-146">This topology randomly generate sentences and count hello occurrence of each word in hello sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="73f79-147">Topológia toohello fürtre történő elküldésekor, át kell másolnia hello használata előtt hello fürtöt tartalmazó jar-fájlra hello `storm` parancsot.</span><span class="sxs-lookup"><span data-stu-id="73f79-147">When submitting topology toohello cluster, you must first copy hello jar file containing hello cluster before using hello `storm` command.</span></span> <span data-ttu-id="73f79-148">toocopy hello toohello fájlfürt, hello használható `scp` parancsot.</span><span class="sxs-lookup"><span data-stu-id="73f79-148">toocopy hello file toohello cluster, you can use hello `scp` command.</span></span> <span data-ttu-id="73f79-149">Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="73f79-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="73f79-150">hello WordCount-példa és egyéb storm starter-példák megtalálhatóak a fürthöz `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="73f79-150">hello WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="73f79-151">Küldje el a topológia: programozott módon</span><span class="sxs-lookup"><span data-stu-id="73f79-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="73f79-152">A HDInsight a topológia tooStorm kommunikál a fürtön tárolt Nimbus szolgáltatás hello programozott módon telepítheti.</span><span class="sxs-lookup"><span data-stu-id="73f79-152">You can programmatically deploy a topology tooStorm on HDInsight by communicating with hello Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="73f79-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) példával Java-alkalmazás, amely bemutatja, hogyan toodeploy és a topológia keresztül hello Nimbus szolgáltatás elindításához.</span><span class="sxs-lookup"><span data-stu-id="73f79-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how toodeploy and start a topology through hello Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="73f79-154">Megfigyelés és kezelés: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73f79-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="73f79-155">Ha egy topológia sikeresen elküldte a Visual Studio használatával, hello **Storm-topológiák** nézetére hello fürt megjelenik-e.</span><span class="sxs-lookup"><span data-stu-id="73f79-155">When a topology has been successfully submitted using Visual Studio, hello **Storm Topologies** view for hello cluster appears.</span></span> <span data-ttu-id="73f79-156">Válassza ki a hello topológiát hello lista tooview információt a futtató topológia hello.</span><span class="sxs-lookup"><span data-stu-id="73f79-156">Select hello topology from hello list tooview information about hello running topology.</span></span>

![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="73f79-158">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="73f79-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="73f79-159">Válassza ki hello alakzat hello spoutok vagy boltok tooview információ ezeket az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="73f79-159">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="73f79-160">Minden elem kijelölve egy új ablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="73f79-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="73f79-161">Inaktiválása és újraaktiválása</span><span class="sxs-lookup"><span data-stu-id="73f79-161">Deactivate and reactivate</span></span>

<span data-ttu-id="73f79-162">A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni.</span><span class="sxs-lookup"><span data-stu-id="73f79-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="73f79-163">tooperform ezeket a műveleteket, használja a hello __Deactivate__ és __újraaktiválása__ hello hello tetején gombok __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="73f79-163">tooperform these operations, use hello __Deactivate__ and __Reactivate__ buttons at hello top of hello __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="73f79-164">Visszaegyensúlyozás</span><span class="sxs-lookup"><span data-stu-id="73f79-164">Rebalance</span></span>

<span data-ttu-id="73f79-165">A topológia újraelosztás lehetővé teszi, hogy a hello rendszer toorevise hello párhuzamosságát hello topológia.</span><span class="sxs-lookup"><span data-stu-id="73f79-165">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="73f79-166">Például ha hello fürt tooadd átméretezte további megjegyzéseket, újraelosztás lehetővé teszi a topológia toosee hello új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="73f79-166">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

<span data-ttu-id="73f79-167">a topológia toorebalance hello használata __Visszaegyensúlyozás__ hello hello tetején gomb __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="73f79-167">toorebalance a topology, use hello __Rebalance__ button at hello top of hello __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="73f79-168">A topológia újraelosztás először inaktiválja hello topológia, munkavállalók egyenletesen újraterjeszti hello fürtön, majd végül adja vissza a hello topológia toohello állapota megelőző újraelosztás történt.</span><span class="sxs-lookup"><span data-stu-id="73f79-168">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="73f79-169">Ezért ha hello topológia már aktív volt, akkor ismét aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="73f79-169">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="73f79-170">Inaktiváltuk, ha a leválasztást inaktív.</span><span class="sxs-lookup"><span data-stu-id="73f79-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="73f79-171">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="73f79-171">Kill a topology</span></span>

<span data-ttu-id="73f79-172">Storm-topológiák továbbra is fut le vannak állítva, vagy hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="73f79-172">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span> <span data-ttu-id="73f79-173">a topológia toostop hello használata __Kill__ hello hello tetején gomb __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="73f79-173">toostop a topology, use hello __Kill__ button at hello top of hello __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-hello-storm-command"></a><span data-ttu-id="73f79-174">Megfigyelés és kezelés: SSH és hello Storm parancs</span><span class="sxs-lookup"><span data-stu-id="73f79-174">Monitor and manage: SSH and hello Storm command</span></span>

<span data-ttu-id="73f79-175">Hello `storm` segédprogram lehetővé teszi a futó topológiákkal hello parancssorból toowork.</span><span class="sxs-lookup"><span data-stu-id="73f79-175">hello `storm` utility allows you toowork with running topologies from hello command line.</span></span> <span data-ttu-id="73f79-176">Használjon `storm -h` parancsok teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="73f79-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="73f79-177">Lista topológiák</span><span class="sxs-lookup"><span data-stu-id="73f79-177">List topologies</span></span>

<span data-ttu-id="73f79-178">A következő parancs toolist hello minden futó topológiákat használhatja:</span><span class="sxs-lookup"><span data-stu-id="73f79-178">Use hello following command toolist all running topologies:</span></span>

    storm list

<span data-ttu-id="73f79-179">Ez a parancs visszaadja a szöveg a következő információk hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="73f79-179">This command returns information similar toohello following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="73f79-180">Inaktiválása és újraaktiválása</span><span class="sxs-lookup"><span data-stu-id="73f79-180">Deactivate and reactivate</span></span>

<span data-ttu-id="73f79-181">A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni.</span><span class="sxs-lookup"><span data-stu-id="73f79-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="73f79-182">A következő parancs toodeactivate hello és és aktiválja újra:</span><span class="sxs-lookup"><span data-stu-id="73f79-182">Use hello following command toodeactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="73f79-183">A futó topológiát leállítása</span><span class="sxs-lookup"><span data-stu-id="73f79-183">Kill a running topology</span></span>

<span data-ttu-id="73f79-184">Storm-topológiák elindításakor, akkor folytassa csak a futó.</span><span class="sxs-lookup"><span data-stu-id="73f79-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="73f79-185">a topológia toostop hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="73f79-185">toostop a topology, use hello following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="73f79-186">Visszaegyensúlyozás</span><span class="sxs-lookup"><span data-stu-id="73f79-186">Rebalance</span></span>

<span data-ttu-id="73f79-187">A topológia újraelosztás lehetővé teszi, hogy a hello rendszer toorevise hello párhuzamosságát hello topológia.</span><span class="sxs-lookup"><span data-stu-id="73f79-187">Rebalancing a topology allows hello system toorevise hello parallelism of hello topology.</span></span> <span data-ttu-id="73f79-188">Például ha hello fürt tooadd átméretezte további megjegyzéseket, újraelosztás lehetővé teszi a topológia toosee hello új csomópontok.</span><span class="sxs-lookup"><span data-stu-id="73f79-188">For example, if you have resized hello cluster tooadd more notes, rebalancing allows a topology toosee hello new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="73f79-189">A topológia újraelosztás először inaktiválja hello topológia, munkavállalók egyenletesen újraterjeszti hello fürtön, majd végül adja vissza a hello topológia toohello állapota megelőző újraelosztás történt.</span><span class="sxs-lookup"><span data-stu-id="73f79-189">Rebalancing a topology first deactivates hello topology, then redistributes workers evenly across hello cluster, then finally returns hello topology toohello state it was in before rebalancing occurred.</span></span> <span data-ttu-id="73f79-190">Ezért ha hello topológia már aktív volt, akkor ismét aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="73f79-190">So if hello topology was active, it becomes active again.</span></span> <span data-ttu-id="73f79-191">Inaktiváltuk, ha a leválasztást inaktív.</span><span class="sxs-lookup"><span data-stu-id="73f79-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="73f79-192">Megfigyelés és kezelés: Storm felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="73f79-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="73f79-193">hello Storm felhasználói felülete webes felületet biztosít a futó topológiákkal dolgozik, és szerepel-e a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="73f79-193">hello Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="73f79-194">tooview hello Storm felhasználói felülete, használjon egy webes böngésző tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, ahol **CLUSTERNAME** hello a fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="73f79-194">tooview hello Storm UI, use a web browser tooopen **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is hello name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="73f79-195">Ha tooprovide kéri a felhasználónevet és jelszót, adja meg a hello Fürtrendszergazda (rendszergazda) és a használt jelszó létrehozása hello fürt.</span><span class="sxs-lookup"><span data-stu-id="73f79-195">If asked tooprovide a user name and password, enter hello cluster administrator (admin) and password that you used when creating hello cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="73f79-196">Főoldala</span><span class="sxs-lookup"><span data-stu-id="73f79-196">Main page</span></span>

<span data-ttu-id="73f79-197">hello fő lapján hello Storm felhasználói felülete hello a következő információkat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="73f79-197">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="73f79-198">**A fürt összefoglaló**: hello Storm-fürt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="73f79-198">**Cluster summary**: Basic information about hello Storm cluster.</span></span>
* <span data-ttu-id="73f79-199">**Összegző topológia**: futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="73f79-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="73f79-200">Ez a szakasz tooview a hello hivatkozások használata az további információt adott topológiák kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="73f79-200">Use hello links in this section tooview more information about specific topologies.</span></span>
* <span data-ttu-id="73f79-201">**Felügyelő összefoglaló**: hello Storm felügyelő kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="73f79-201">**Supervisor summary**: Information about hello Storm supervisor.</span></span>
* <span data-ttu-id="73f79-202">**Nimbus konfigurációs**: hello fürt Nimbus konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="73f79-202">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="73f79-203">Topológia összegzése</span><span class="sxs-lookup"><span data-stu-id="73f79-203">Topology summary</span></span>

<span data-ttu-id="73f79-204">Egy hivatkozási kiválasztása a hello **topológia összegzése** szakasz hello topológiára vonatkozó adatokat a következő hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="73f79-204">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="73f79-205">**Összegző topológia**: hello topológia alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="73f79-205">**Topology summary**: Basic information about hello topology.</span></span>
* <span data-ttu-id="73f79-206">**Topológia műveletek**: hello topológia végrehajtható felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="73f79-206">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="73f79-207">**Aktiválása**: folytatása inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="73f79-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="73f79-208">**Inaktiválása**: megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="73f79-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="73f79-209">**Visszaegyensúlyozás**: hello topológia párhuzamosságát hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="73f79-209">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="73f79-210">Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello.</span><span class="sxs-lookup"><span data-stu-id="73f79-210">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="73f79-211">Ez a művelet lehetővé teszi, hogy hello topológia tooadjust párhuzamossági toocompensate hello növelhető vagy csökkenthető, hello fürtben található csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="73f79-211">This operation allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

    <span data-ttu-id="73f79-212">További információkért lásd: <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">ismertetése a Storm-topológia párhuzamosságát hello</a>.</span><span class="sxs-lookup"><span data-stu-id="73f79-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding hello parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="73f79-213">**Kill**: a Storm-topológia leállítása után hello megadott időkorlát.</span><span class="sxs-lookup"><span data-stu-id="73f79-213">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>
* <span data-ttu-id="73f79-214">**Topológiastatisztikák**: hello topológia statisztikája.</span><span class="sxs-lookup"><span data-stu-id="73f79-214">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="73f79-215">tooset hello hello oldalon bejegyzések fennmaradó hello határideje, hello hello hivatkozásokkal **ablak** oszlop.</span><span class="sxs-lookup"><span data-stu-id="73f79-215">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="73f79-216">**Spoutok**: hello hello topológia által használt spoutokkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="73f79-216">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="73f79-217">Ez a szakasz tooview a hello hivatkozások használata az adott spoutokkal kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="73f79-217">Use hello links in this section tooview more information about specific spouts.</span></span>
* <span data-ttu-id="73f79-218">**Boltok**: hello boltokhoz hello topológia használják.</span><span class="sxs-lookup"><span data-stu-id="73f79-218">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="73f79-219">Ez a szakasz tooview a hello hivatkozások használata az adott boltokhoz további információt.</span><span class="sxs-lookup"><span data-stu-id="73f79-219">Use hello links in this section tooview more information about specific bolts.</span></span>
* <span data-ttu-id="73f79-220">**Topológiakonfiguráció**: hello hello kiválasztott topológia beállításától.</span><span class="sxs-lookup"><span data-stu-id="73f79-220">**Topology configuration**: hello configuration of hello selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="73f79-221">Spout és Bolt összegzése</span><span class="sxs-lookup"><span data-stu-id="73f79-221">Spout and Bolt summary</span></span>

<span data-ttu-id="73f79-222">Egy spout kijelölésével hello **Spoutok** vagy **boltok** szakaszok hello kijelölt elemre vonatkozó információkat a következő hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="73f79-222">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="73f79-223">**Az összetevő összegzés**: hello spout vagy bolt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="73f79-223">**Component summary**: Basic information about hello spout or bolt.</span></span>
* <span data-ttu-id="73f79-224">**Spout vagy Bolt statisztikák**: hello statisztikája spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="73f79-224">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="73f79-225">tooset hello hello oldalon bejegyzések fennmaradó hello határideje, hello hello hivatkozásokkal **ablak** oszlop.</span><span class="sxs-lookup"><span data-stu-id="73f79-225">tooset hello timeframe for hello remaining entries on hello page, use hello links in hello **Window** column.</span></span>
* <span data-ttu-id="73f79-226">**Beviteli statisztikák** (csak boltok esetében): hello információ bemenetét hello bolt által használt.</span><span class="sxs-lookup"><span data-stu-id="73f79-226">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>
* <span data-ttu-id="73f79-227">**Kimeneti statisztikák**: hello adatfolyamok által kibocsátott információ spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="73f79-227">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="73f79-228">**Végrehajtója**: hello spout vagy bolt hello példányaira vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="73f79-228">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="73f79-229">Jelölje be hello **Port** egy adott végrehajtó tooview bejegyzést a diagnosztikai adatokat tartalmazó naplófájlt előállított erre a példányra.</span><span class="sxs-lookup"><span data-stu-id="73f79-229">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="73f79-230">**Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="73f79-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="73f79-231">Megfigyelés és kezelés: REST API-n</span><span class="sxs-lookup"><span data-stu-id="73f79-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="73f79-232">hello Storm felhasználói felülete hello REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, hello REST API használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="73f79-232">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="73f79-233">Felügyelheti és figyelheti a Storm-topológiák hello REST API toocreate egyéni eszközöket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="73f79-233">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="73f79-234">További információkért lásd: [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="73f79-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="73f79-235">hello következő információkat az adott toousing hello REST API-t a HDInsight alatt futó Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="73f79-235">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="73f79-236">nincs nyilvánosan elérhető hello Storm REST API-t több mint hello internet, és egy SSH alagút toohello HDInsight fürt átjárócsomópontjába használatával kell elérni.</span><span class="sxs-lookup"><span data-stu-id="73f79-236">hello Storm REST API is not publicly available over hello internet, and must be accessed using an SSH tunnel toohello HDInsight cluster head node.</span></span> <span data-ttu-id="73f79-237">Létrehozásával és az SSH-alagút használatával további információkért lásd: [használata SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie és egyéb web UI](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling tooaccess Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="73f79-238">Alap URI</span><span class="sxs-lookup"><span data-stu-id="73f79-238">Base URI</span></span>

<span data-ttu-id="73f79-239">hello alap URI-Azonosítójának a Linux-alapú HDInsight-fürtök REST API hello érhető el: hello átjárócsomópont **https://HEADNODEFQDN:8744/api/v1/**.</span><span class="sxs-lookup"><span data-stu-id="73f79-239">hello base URI for hello REST API on Linux-based HDInsight clusters is available on hello head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="73f79-240">hello átjárócsomópont hello tartománynevet jön létre a fürt létrehozása során, és nincs statikus.</span><span class="sxs-lookup"><span data-stu-id="73f79-240">hello domain name of hello head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="73f79-241">A különféle módokon található hello teljesen minősített tartománynevét (FQDN) hello átjárócsomóponthoz:</span><span class="sxs-lookup"><span data-stu-id="73f79-241">You can find hello fully qualified domain name (FQDN) for hello cluster head node in several different ways:</span></span>

* <span data-ttu-id="73f79-242">**Az SSH-munkamenetet**: hello paranccsal `headnode -f` egy SSH-munkamenet toohello fürtből.</span><span class="sxs-lookup"><span data-stu-id="73f79-242">**From an SSH session**: Use hello command `headnode -f` from an SSH session toohello cluster.</span></span>
* <span data-ttu-id="73f79-243">**Az Ambari webes**: válasszon **szolgáltatások** hello hello oldal tetejére, majd válassza ki **Storm**.</span><span class="sxs-lookup"><span data-stu-id="73f79-243">**From Ambari Web**: Select **Services** from hello top of hello page, then select **Storm**.</span></span> <span data-ttu-id="73f79-244">A hello **összegzés** lapon jelölje be **Storm felhasználói felület Server**.</span><span class="sxs-lookup"><span data-stu-id="73f79-244">From hello **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="73f79-245">hello hello Storm felhasználói felülete és a REST API-t futtató hello csomópont teljes Tartományneve van hello oldal hello tetején.</span><span class="sxs-lookup"><span data-stu-id="73f79-245">hello FQDN of hello node that hello Storm UI and REST API is running is at hello top of hello page.</span></span>
* <span data-ttu-id="73f79-246">**Az Ambari REST API**: hello paranccsal `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve információ a Storm felhasználói felület és a REST API hello hello csomópontot futtat.</span><span class="sxs-lookup"><span data-stu-id="73f79-246">**From Ambari REST API**: Use hello command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` tooretrieve information about hello node that hello Storm UI and REST API are running on.</span></span> <span data-ttu-id="73f79-247">Cserélje le **jelszó** hello fürt hello rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="73f79-247">Replace **PASSWORD** with hello admin password for hello cluster.</span></span> <span data-ttu-id="73f79-248">Cserélje le **CLUSTERNAME** hello fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="73f79-248">Replace **CLUSTERNAME** with hello cluster name.</span></span> <span data-ttu-id="73f79-249">Hello válaszul hello "gazdaszámítógép_neve" bejegyzés tartalmazza hello hello csomópont teljesen minősített Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="73f79-249">In hello response, hello "host_name" entry contains hello FQDN of hello node.</span></span>

### <a name="authentication"></a><span data-ttu-id="73f79-250">Authentication</span><span class="sxs-lookup"><span data-stu-id="73f79-250">Authentication</span></span>

<span data-ttu-id="73f79-251">REST API-t kell használnia kérelmek toohello **az egyszerű hitelesítés**, így a hello HDInsight fürt rendszergazdája nevét és jelszavát használja.</span><span class="sxs-lookup"><span data-stu-id="73f79-251">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="73f79-252">Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** hello fürt toosecure HTTPS-kommunikációhoz használni.</span><span class="sxs-lookup"><span data-stu-id="73f79-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="73f79-253">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="73f79-253">Return values</span></span>

<span data-ttu-id="73f79-254">REST API-t csak lehet hello fürtön belül használható, vagy a virtuális gépek azonos Azure Virtual Network hello fürtként hello hello visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="73f79-254">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="73f79-255">Például hello teljesen minősített tartománynevét (FQDN) Zookeeper kiszolgálók visszaadott értéke nem érhető el hello Internet.</span><span class="sxs-lookup"><span data-stu-id="73f79-255">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73f79-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="73f79-256">Next Steps</span></span>

<span data-ttu-id="73f79-257">Most, hogy megismerte a hogyan toodeploy és a figyelő topológiák használatával hello a Storm irányítópultja, megtudhatja, hogyan túl[Maven használatával fejlesztése Java-alapú topológiák](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-257">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how too[Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="73f79-258">További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="73f79-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
