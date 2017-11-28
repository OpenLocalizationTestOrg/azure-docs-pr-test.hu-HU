---
title: "Központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan telepítheti, figyelheti és kezelheti a Storm irányítópultjának használata a Linux-alapú HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
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
ms.openlocfilehash: b9e82463030807d2674594e73f762fe93515d423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-hdinsight"></a><span data-ttu-id="6d653-104">Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="6d653-104">Deploy and manage Apache Storm topologies on HDInsight</span></span>

<span data-ttu-id="6d653-105">Ebben a dokumentumban alapismeretei kezelése és figyelése a Storm a HDInsight-fürtökön futó Storm-topológiák.</span><span class="sxs-lookup"><span data-stu-id="6d653-105">In this document, learn the basics of managing and monitoring Storm topologies running on Storm on HDInsight clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d653-106">A cikkben leírt lépéseket a Linux-alapú Storm on HDInsight-fürt szükséges.</span><span class="sxs-lookup"><span data-stu-id="6d653-106">The steps in this article require a Linux-based Storm on HDInsight cluster.</span></span> <span data-ttu-id="6d653-107">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="6d653-107">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6d653-108">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6d653-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> 
>
> <span data-ttu-id="6d653-109">A telepítését és megfigyelését futó Windows-alapú HDInsight-topológiák további információkért lásd: [központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology.md)</span><span class="sxs-lookup"><span data-stu-id="6d653-109">For information on deploying and monitoring topologies on Windows-based HDInsight, see [Deploy and manage Apache Storm topologies on Windows-based HDInsight](hdinsight-storm-deploy-monitor-topology.md)</span></span>


## <a name="prerequisites"></a><span data-ttu-id="6d653-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6d653-110">Prerequisites</span></span>

* <span data-ttu-id="6d653-111">**A Linux-alapú Storm on HDInsight-fürt**: lásd: [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started-linux.md) fürt létrehozásának lépései</span><span class="sxs-lookup"><span data-stu-id="6d653-111">**A Linux-based Storm on HDInsight cluster**: see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started-linux.md) for steps on creating a cluster</span></span>

* <span data-ttu-id="6d653-112">(Választható) **SSH és az SCP ismeretét**: további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-112">(Optional) **Familiarity with SSH and SCP**: For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="6d653-113">(Választható) **Visual Studio**: Azure SDK 2.5.1-es vagy újabb és a Data Lake Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d653-113">(Optional) **Visual Studio**: Azure SDK 2.5.1 or newer and the Data Lake Tools for Visual Studio.</span></span> <span data-ttu-id="6d653-114">További információkért lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-114">For more information, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    <span data-ttu-id="6d653-115">A Visual Studio következő verzióinak valamelyike:</span><span class="sxs-lookup"><span data-stu-id="6d653-115">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="6d653-116">A Visual Studio 2012 [4. frissítés](http://www.microsoft.com/download/details.aspx?id=39305)</span><span class="sxs-lookup"><span data-stu-id="6d653-116">Visual Studio 2012 with [Update 4](http://www.microsoft.com/download/details.aspx?id=39305)</span></span>

  * <span data-ttu-id="6d653-117">A Visual Studio 2013-as verziójának [4. frissítés](http://www.microsoft.com/download/details.aspx?id=44921) vagy [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span><span class="sxs-lookup"><span data-stu-id="6d653-117">Visual Studio 2013 with [Update 4](http://www.microsoft.com/download/details.aspx?id=44921) or [Visual Studio 2013 Community](http://go.microsoft.com/fwlink/?LinkId=517284)</span></span>
  * [<span data-ttu-id="6d653-118">A Visual Studio 2015</span><span class="sxs-lookup"><span data-stu-id="6d653-118">Visual Studio 2015</span></span>](https://www.visualstudio.com/downloads/)

  * <span data-ttu-id="6d653-119">A Visual Studio 2015 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="6d653-119">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="6d653-120">A Visual Studio 2017 (minden kiadás).</span><span class="sxs-lookup"><span data-stu-id="6d653-120">Visual Studio 2017 (any edition).</span></span> <span data-ttu-id="6d653-121">A Data Lake Tools for Visual Studio 2017 Azure tárterületet részeként telepített.</span><span class="sxs-lookup"><span data-stu-id="6d653-121">Data Lake Tools for Visual Studio 2017 are installed as part of the Azure Workload.</span></span>

## <a name="submit-a-topology-visual-studio"></a><span data-ttu-id="6d653-122">Küldje el a topológia: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d653-122">Submit a topology: Visual Studio</span></span>

<span data-ttu-id="6d653-123">A HDInsight Tools elküldeni a C# vagy hibrid topológiák a Storm fürthöz használható.</span><span class="sxs-lookup"><span data-stu-id="6d653-123">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="6d653-124">Az alábbi lépéseket a mintaalkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="6d653-124">The following steps use a sample application.</span></span> <span data-ttu-id="6d653-125">A HDInsight Tools használatával saját topológiák létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák a HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-125">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

1. <span data-ttu-id="6d653-126">Ha még nem telepítette a legújabb verzióját a Data Lake tools for Visual Studio, lásd: [Ismerkedés a Data Lake Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-126">If you have not already installed the latest version of the Data Lake tools for Visual Studio, see [Get started using Data Lake Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="6d653-127">A Data Lake Tools for Visual Studio volt korábbi nevén a HDInsight Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6d653-127">The Data Lake Tools for Visual Studio were formerly called the HDInsight Tools for Visual Studio.</span></span>
    >
    > <span data-ttu-id="6d653-128">A Data Lake Tools for Visual Studio szerepelnek a __Azure munkaterhelés__ a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="6d653-128">Data Lake Tools for Visual Studio are included in the __Azure Workload__ for Visual Studio 2017.</span></span>

2. <span data-ttu-id="6d653-129">Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="6d653-129">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="6d653-130">Az a **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="6d653-130">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="6d653-131">Válassza ki a listáról a sablonok **Storm minta**.</span><span class="sxs-lookup"><span data-stu-id="6d653-131">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="6d653-132">A párbeszédpanel alján írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="6d653-132">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology-linux/sample.png)

4. <span data-ttu-id="6d653-134">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="6d653-134">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6d653-135">Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatok az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="6d653-135">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="6d653-136">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be, amely tartalmazza a Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="6d653-136">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="6d653-137">Válassza ki a Storm on HDInsight-fürt a a **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="6d653-137">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="6d653-138">-E a küldése sikeres használatával figyelheti a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="6d653-138">You can monitor whether the submission is successful by using the **Output** window.</span></span>

## <a name="submit-a-topology-ssh-and-the-storm-command"></a><span data-ttu-id="6d653-139">Küldje el a topológia: SSH és a Storm parancs</span><span class="sxs-lookup"><span data-stu-id="6d653-139">Submit a topology: SSH and the Storm command</span></span>

1. <span data-ttu-id="6d653-140">Az SSH használata a HDInsight-fürthöz való csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="6d653-140">Use SSH to connect to the HDInsight cluster.</span></span> <span data-ttu-id="6d653-141">Cserélje le **felhasználónév** az SSH-bejelentkezéskor nevét.</span><span class="sxs-lookup"><span data-stu-id="6d653-141">Replace **USERNAME** the name of your SSH login.</span></span> <span data-ttu-id="6d653-142">Cserélje le **CLUSTERNAME** a HDInsight fürt nevű:</span><span class="sxs-lookup"><span data-stu-id="6d653-142">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    <span data-ttu-id="6d653-143">További információ az SSH használatával kapcsolódni HDInsight-fürtjéhez, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-143">For more information on using SSH to connect to your HDInsight cluster, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="6d653-144">Használja az alábbi parancsot példatopológia indításához:</span><span class="sxs-lookup"><span data-stu-id="6d653-144">Use the following command to start an example topology:</span></span>

        storm jar /usr/hdp/current/storm-client/contrib/storm-starter/storm-starter-topologies-*.jar org.apache.storm.starter.WordCountTopology WordCount

    <span data-ttu-id="6d653-145">A paranccsal elindítja a példa WordCount-topológia a fürtön.</span><span class="sxs-lookup"><span data-stu-id="6d653-145">This command starts the example WordCount topology on the cluster.</span></span> <span data-ttu-id="6d653-146">Ez a topológia véletlenszerűen generálja a mondatok, és az összes szó a mondatok előfordulási száma.</span><span class="sxs-lookup"><span data-stu-id="6d653-146">This topology randomly generate sentences and count the occurrence of each word in the sentences.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6d653-147">A topológia a fürtre történő elküldésekor a fürtöt tartalmazó jar-fájlt a `storm` parancs használata előtt kell másolnia.</span><span class="sxs-lookup"><span data-stu-id="6d653-147">When submitting topology to the cluster, you must first copy the jar file containing the cluster before using the `storm` command.</span></span> <span data-ttu-id="6d653-148">A fájl átmásolása a fürtre, használja a `scp` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6d653-148">To copy the file to the cluster, you can use the `scp` command.</span></span> <span data-ttu-id="6d653-149">Például: `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span><span class="sxs-lookup"><span data-stu-id="6d653-149">For example, `scp FILENAME.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:FILENAME.jar`</span></span>
   >
   > <span data-ttu-id="6d653-150">A WordCount-példa és egyéb Storm Starter-példák megtalálhatóak a fürtön a következő helyen: `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span><span class="sxs-lookup"><span data-stu-id="6d653-150">The WordCount example, and other storm starter examples, are already included on your cluster at `/usr/hdp/current/storm-client/contrib/storm-starter/`.</span></span>

## <a name="submit-a-topology-programmatically"></a><span data-ttu-id="6d653-151">Küldje el a topológia: programozott módon</span><span class="sxs-lookup"><span data-stu-id="6d653-151">Submit a topology: programmatically</span></span>

<span data-ttu-id="6d653-152">A topológia a HDInsight alatt futó Storm által a a fürtön futó Nimbus szolgáltatással folytatott kommunikáció programozott módon telepítheti.</span><span class="sxs-lookup"><span data-stu-id="6d653-152">You can programmatically deploy a topology to Storm on HDInsight by communicating with the Nimbus service hosted in your cluster.</span></span> <span data-ttu-id="6d653-153">[https://github.com/Azure-Samples/hdinsight-Java-Deploy-Storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) példával Java-alkalmazás, amely bemutatja, hogyan kell telepíteni és elindítani a topológia a Nimbus szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="6d653-153">[https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology](https://github.com/Azure-Samples/hdinsight-java-deploy-storm-topology) provides an example Java application that demonstrates how to deploy and start a topology through the Nimbus service.</span></span>

## <a name="monitor-and-manage-visual-studio"></a><span data-ttu-id="6d653-154">Megfigyelés és kezelés: Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6d653-154">Monitor and manage: Visual Studio</span></span>

<span data-ttu-id="6d653-155">Ha egy topológia sikeresen elküldte a Visual Studio használatával a **Storm-topológiák** nézetére a fürt megjelenik-e.</span><span class="sxs-lookup"><span data-stu-id="6d653-155">When a topology has been successfully submitted using Visual Studio, the **Storm Topologies** view for the cluster appears.</span></span> <span data-ttu-id="6d653-156">Válassza ki a topológiát a futó topológiát vonatkozó információk megtekintése a listából.</span><span class="sxs-lookup"><span data-stu-id="6d653-156">Select the topology from the list to view information about the running topology.</span></span>

![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

> [!NOTE]
> <span data-ttu-id="6d653-158">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="6d653-158">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

<span data-ttu-id="6d653-159">Válassza ki az alakzat a spoutokkal kapcsolatban, vagy a boltokhoz ezeket az összetevőket vonatkozó információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="6d653-159">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="6d653-160">Minden elem kijelölve egy új ablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="6d653-160">A new window opens for each item selected.</span></span>

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="6d653-161">Inaktiválása és újraaktiválása</span><span class="sxs-lookup"><span data-stu-id="6d653-161">Deactivate and reactivate</span></span>

<span data-ttu-id="6d653-162">A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni.</span><span class="sxs-lookup"><span data-stu-id="6d653-162">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="6d653-163">Ezek a műveletek végrehajtásához használja a __Deactivate__ és __újraaktiválása__ gombok tetején a __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="6d653-163">To perform these operations, use the __Deactivate__ and __Reactivate__ buttons at the top of the __Topology Summary__.</span></span>

### <a name="rebalance"></a><span data-ttu-id="6d653-164">Visszaegyensúlyozás</span><span class="sxs-lookup"><span data-stu-id="6d653-164">Rebalance</span></span>

<span data-ttu-id="6d653-165">A topológia újraelosztás lehetővé teszi, hogy a rendszer a topológia párhuzamosságát ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d653-165">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="6d653-166">Például ha a fürt további megjegyzéseket hozzáadni átméretezte, újraelosztás lehetővé teszi, hogy tekintse meg az új csomópontok topológiát.</span><span class="sxs-lookup"><span data-stu-id="6d653-166">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

<span data-ttu-id="6d653-167">A topológia egyensúlyba, használja a __Visszaegyensúlyozás__ gomb tetején a __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="6d653-167">To rebalance a topology, use the __Rebalance__ button at the top of the __Topology Summary__.</span></span>

> [!WARNING]
> <span data-ttu-id="6d653-168">A topológia újraelosztás először inaktiválja a topológia munkavállalók egyenletesen újraterjeszti a fürtön, majd végül a újraelosztás történt előtti állapotba tér vissza a topológia.</span><span class="sxs-lookup"><span data-stu-id="6d653-168">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="6d653-169">Ezért ha a topológia már aktív volt, akkor ismét aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="6d653-169">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="6d653-170">Inaktiváltuk, ha a leválasztást inaktív.</span><span class="sxs-lookup"><span data-stu-id="6d653-170">If it was deactivated, it remains deactivated.</span></span>

### <a name="kill-a-topology"></a><span data-ttu-id="6d653-171">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="6d653-171">Kill a topology</span></span>

<span data-ttu-id="6d653-172">Storm-topológiák továbbra is fut le vannak állítva, vagy a fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="6d653-172">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span> <span data-ttu-id="6d653-173">A topológia leállításához használja a __Kill__ gomb tetején a __topológia összegzése__.</span><span class="sxs-lookup"><span data-stu-id="6d653-173">To stop a topology, use the __Kill__ button at the top of the __Topology Summary__.</span></span>

## <a name="monitor-and-manage-ssh-and-the-storm-command"></a><span data-ttu-id="6d653-174">Megfigyelés és kezelés: SSH és a Storm parancs</span><span class="sxs-lookup"><span data-stu-id="6d653-174">Monitor and manage: SSH and the Storm command</span></span>

<span data-ttu-id="6d653-175">A `storm` segédprogram lehetővé teszi a futó topológiákkal a parancssorból.</span><span class="sxs-lookup"><span data-stu-id="6d653-175">The `storm` utility allows you to work with running topologies from the command line.</span></span> <span data-ttu-id="6d653-176">Használjon `storm -h` parancsok teljes listáját.</span><span class="sxs-lookup"><span data-stu-id="6d653-176">Use `storm -h` for a full list of commands.</span></span>

### <a name="list-topologies"></a><span data-ttu-id="6d653-177">Lista topológiák</span><span class="sxs-lookup"><span data-stu-id="6d653-177">List topologies</span></span>

<span data-ttu-id="6d653-178">Használja a következő parancs futtatásával listázza az összes futó topológiákkal:</span><span class="sxs-lookup"><span data-stu-id="6d653-178">Use the following command to list all running topologies:</span></span>

    storm list

<span data-ttu-id="6d653-179">Ez a parancs az alábbi szöveghez hasonló információt ad vissza:</span><span class="sxs-lookup"><span data-stu-id="6d653-179">This command returns information similar to the following text:</span></span>

    Topology_name        Status     Num_tasks  Num_workers  Uptime_secs
    -------------------------------------------------------------------
    WordCount            ACTIVE     29         2            263

### <a name="deactivate-and-reactivate"></a><span data-ttu-id="6d653-180">Inaktiválása és újraaktiválása</span><span class="sxs-lookup"><span data-stu-id="6d653-180">Deactivate and reactivate</span></span>

<span data-ttu-id="6d653-181">A topológia inaktiválása felfüggeszti, amíg következtében leállt, vagy újra aktiválni.</span><span class="sxs-lookup"><span data-stu-id="6d653-181">Deactivating a topology pauses it until it is killed or reactivated.</span></span> <span data-ttu-id="6d653-182">A következő paranccsal inaktiválása és újraaktiválása:</span><span class="sxs-lookup"><span data-stu-id="6d653-182">Use the following command to deactivate and reactivate:</span></span>

    storm Deactivate TOPOLOGYNAME

    storm Activate TOPOLOGYNAME

### <a name="kill-a-running-topology"></a><span data-ttu-id="6d653-183">A futó topológiát leállítása</span><span class="sxs-lookup"><span data-stu-id="6d653-183">Kill a running topology</span></span>

<span data-ttu-id="6d653-184">Storm-topológiák elindításakor, akkor folytassa csak a futó.</span><span class="sxs-lookup"><span data-stu-id="6d653-184">Storm topologies, once started, continue running until stopped.</span></span> <span data-ttu-id="6d653-185">A topológia leállítása, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="6d653-185">To stop a topology, use the following command:</span></span>

    storm kill TOPOLOGYNAME

### <a name="rebalance"></a><span data-ttu-id="6d653-186">Visszaegyensúlyozás</span><span class="sxs-lookup"><span data-stu-id="6d653-186">Rebalance</span></span>

<span data-ttu-id="6d653-187">A topológia újraelosztás lehetővé teszi, hogy a rendszer a topológia párhuzamosságát ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="6d653-187">Rebalancing a topology allows the system to revise the parallelism of the topology.</span></span> <span data-ttu-id="6d653-188">Például ha a fürt további megjegyzéseket hozzáadni átméretezte, újraelosztás lehetővé teszi, hogy tekintse meg az új csomópontok topológiát.</span><span class="sxs-lookup"><span data-stu-id="6d653-188">For example, if you have resized the cluster to add more notes, rebalancing allows a topology to see the new nodes.</span></span>

> [!WARNING]
> <span data-ttu-id="6d653-189">A topológia újraelosztás először inaktiválja a topológia munkavállalók egyenletesen újraterjeszti a fürtön, majd végül a újraelosztás történt előtti állapotba tér vissza a topológia.</span><span class="sxs-lookup"><span data-stu-id="6d653-189">Rebalancing a topology first deactivates the topology, then redistributes workers evenly across the cluster, then finally returns the topology to the state it was in before rebalancing occurred.</span></span> <span data-ttu-id="6d653-190">Ezért ha a topológia már aktív volt, akkor ismét aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="6d653-190">So if the topology was active, it becomes active again.</span></span> <span data-ttu-id="6d653-191">Inaktiváltuk, ha a leválasztást inaktív.</span><span class="sxs-lookup"><span data-stu-id="6d653-191">If it was deactivated, it remains deactivated.</span></span>

    storm rebalance TOPOLOGYNAME

## <a name="monitor-and-manage-storm-ui"></a><span data-ttu-id="6d653-192">Megfigyelés és kezelés: Storm felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="6d653-192">Monitor and manage: Storm UI</span></span>

<span data-ttu-id="6d653-193">A HDInsight-fürtön elérhető Storm webes felhasználói felületet biztosít a futó topológiákkal való munkavégzéshez.</span><span class="sxs-lookup"><span data-stu-id="6d653-193">The Storm UI provides a web interface for working with running topologies, and is included on your HDInsight cluster.</span></span> <span data-ttu-id="6d653-194">A Storm felhasználói felülete segítségével tekintheti egy webes böngésző megnyitásához **https://CLUSTERNAME.azurehdinsight.net/stormui**, ahol **CLUSTERNAME** a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="6d653-194">To view the Storm UI, use a web browser to open **https://CLUSTERNAME.azurehdinsight.net/stormui**, where **CLUSTERNAME** is the name of your cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="6d653-195">Ha a rendszer felkéri a felhasználónév és a jelszó megadására, a fürt létrehozásakor használt fürtrendszergazda (rendszergazda) nevét és jelszavát adja meg.</span><span class="sxs-lookup"><span data-stu-id="6d653-195">If asked to provide a user name and password, enter the cluster administrator (admin) and password that you used when creating the cluster.</span></span>

### <a name="main-page"></a><span data-ttu-id="6d653-196">Főoldala</span><span class="sxs-lookup"><span data-stu-id="6d653-196">Main page</span></span>

<span data-ttu-id="6d653-197">A Storm felhasználói felület a fő lapján az alábbi információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6d653-197">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="6d653-198">**A fürt összefoglaló**: a Storm-fürt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="6d653-198">**Cluster summary**: Basic information about the Storm cluster.</span></span>
* <span data-ttu-id="6d653-199">**Összegző topológia**: futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="6d653-199">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="6d653-200">Ebben a szakaszban a hivatkozások segítségével további információkat szeretne az adott topológiákat.</span><span class="sxs-lookup"><span data-stu-id="6d653-200">Use the links in this section to view more information about specific topologies.</span></span>
* <span data-ttu-id="6d653-201">**Összegző felügyelő**: a Storm felügyelő kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="6d653-201">**Supervisor summary**: Information about the Storm supervisor.</span></span>
* <span data-ttu-id="6d653-202">**Nimbus konfigurációs**: Nimbus konfigurációját a fürtben.</span><span class="sxs-lookup"><span data-stu-id="6d653-202">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

### <a name="topology-summary"></a><span data-ttu-id="6d653-203">Topológia összegzése</span><span class="sxs-lookup"><span data-stu-id="6d653-203">Topology summary</span></span>

<span data-ttu-id="6d653-204">A hivatkozás kiválasztása a **topológia összegzése** csoportban megjelennek a topológia a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6d653-204">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="6d653-205">**Összegző topológia**: a topológia alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="6d653-205">**Topology summary**: Basic information about the topology.</span></span>
* <span data-ttu-id="6d653-206">**Topológia műveletek**: kezelési műveleteket hajthat végre a topológia.</span><span class="sxs-lookup"><span data-stu-id="6d653-206">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="6d653-207">**Aktiválása**: folytatása inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="6d653-207">**Activate**: Resumes processing of a deactivated topology.</span></span>
  * <span data-ttu-id="6d653-208">**Inaktiválása**: megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="6d653-208">**Deactivate**: Pauses a running topology.</span></span>
  * <span data-ttu-id="6d653-209">**Visszaegyensúlyozás**: Beállítja a topológia párhuzamosságát.</span><span class="sxs-lookup"><span data-stu-id="6d653-209">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="6d653-210">A fürtben található csomópontok számának megváltoztatását követően újra ki kell egyensúlyozni a futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="6d653-210">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="6d653-211">Ez a művelet lehetővé teszi, hogy a topológia párhuzamosságának kiegyensúlyozása érdekében a növelő vagy csökkentő a fürtben található csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="6d653-211">This operation allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

    <span data-ttu-id="6d653-212">További információ: <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology</a> (A Storm-topológia párhuzamosságának ismertetése).</span><span class="sxs-lookup"><span data-stu-id="6d653-212">For more information, see <a href="http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html" target="_blank">Understanding the parallelism of a Storm topology</a>.</span></span>
  * <span data-ttu-id="6d653-213">**Kill**: a Storm-topológia leállítása bizonyos időtúllépést követően.</span><span class="sxs-lookup"><span data-stu-id="6d653-213">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>
* <span data-ttu-id="6d653-214">**Topológiastatisztikák**: a topológia statisztikája.</span><span class="sxs-lookup"><span data-stu-id="6d653-214">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="6d653-215">Az oldalon időkeretének megadására a többieknek beállításához használja a hivatkozások a **ablak** oszlop.</span><span class="sxs-lookup"><span data-stu-id="6d653-215">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="6d653-216">**Spoutok**: A spoutokkal kapcsolatban, használja a topológiát.</span><span class="sxs-lookup"><span data-stu-id="6d653-216">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="6d653-217">Ebben a szakaszban a hivatkozások segítségével megtekintheti az adott spoutokkal kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="6d653-217">Use the links in this section to view more information about specific spouts.</span></span>
* <span data-ttu-id="6d653-218">**Boltok**: A boltokhoz a topológia használják.</span><span class="sxs-lookup"><span data-stu-id="6d653-218">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="6d653-219">Ebben a szakaszban található hivatkozások segítségével további információkat szeretne az adott boltokhoz.</span><span class="sxs-lookup"><span data-stu-id="6d653-219">Use the links in this section to view more information about specific bolts.</span></span>
* <span data-ttu-id="6d653-220">**Topológiakonfiguráció**: a kiválasztott topológia beállításától.</span><span class="sxs-lookup"><span data-stu-id="6d653-220">**Topology configuration**: The configuration of the selected topology.</span></span>

### <a name="spout-and-bolt-summary"></a><span data-ttu-id="6d653-221">Spout és Bolt összegzése</span><span class="sxs-lookup"><span data-stu-id="6d653-221">Spout and Bolt summary</span></span>

<span data-ttu-id="6d653-222">Az egy spout kiválasztása a **Spoutok** vagy **boltok** szakaszok megjeleníti a kijelölt elem a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="6d653-222">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="6d653-223">**Az összetevő összegzés**: a spout vagy bolt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="6d653-223">**Component summary**: Basic information about the spout or bolt.</span></span>
* <span data-ttu-id="6d653-224">**Spout vagy Bolt statisztikák**: a spout vagy bolt statisztikája.</span><span class="sxs-lookup"><span data-stu-id="6d653-224">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="6d653-225">Az oldalon időkeretének megadására a többieknek beállításához használja a hivatkozások a **ablak** oszlop.</span><span class="sxs-lookup"><span data-stu-id="6d653-225">To set the timeframe for the remaining entries on the page, use the links in the **Window** column.</span></span>
* <span data-ttu-id="6d653-226">**Beviteli statisztikák** (csak boltok esetében): a bemeneti adatfolyamot tartalmaz a bolt által felhasznált információk.</span><span class="sxs-lookup"><span data-stu-id="6d653-226">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>
* <span data-ttu-id="6d653-227">**Kimeneti statisztikák**: az adatfolyamokat, ez által kibocsátott információ spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="6d653-227">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>
* <span data-ttu-id="6d653-228">**Végrehajtója**: a spout vagy bolt példányaira vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="6d653-228">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="6d653-229">Válassza ki a **Port** egy adott végrehajtó diagnosztikai adatok megtekintését, a bejegyzés erre a példányra.</span><span class="sxs-lookup"><span data-stu-id="6d653-229">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>
* <span data-ttu-id="6d653-230">**Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="6d653-230">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="monitor-and-manage-rest-api"></a><span data-ttu-id="6d653-231">Megfigyelés és kezelés: REST API-n</span><span class="sxs-lookup"><span data-stu-id="6d653-231">Monitor and manage: REST API</span></span>

<span data-ttu-id="6d653-232">A Storm felhasználói felülete a REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, a REST API használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="6d653-232">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="6d653-233">A REST API-t hozhat létre egyéni eszközök felügyelheti és figyelheti a Storm-topológiák.</span><span class="sxs-lookup"><span data-stu-id="6d653-233">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="6d653-234">További információkért lásd: [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span><span class="sxs-lookup"><span data-stu-id="6d653-234">For more information, see [Storm UI REST API](http://storm.apache.org/releases/0.9.6/STORM-UI-REST-API.html).</span></span> <span data-ttu-id="6d653-235">Az alábbi információk csak a HDInsight alatt futó Apache Storm a REST API használatával az elő.</span><span class="sxs-lookup"><span data-stu-id="6d653-235">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d653-236">A Storm REST API nincs nyilvánosan elérhető az interneten keresztül, és a HDInsight fürt átjárócsomópontjába az SSH-alagúton keresztül kell elérni.</span><span class="sxs-lookup"><span data-stu-id="6d653-236">The Storm REST API is not publicly available over the internet, and must be accessed using an SSH tunnel to the HDInsight cluster head node.</span></span> <span data-ttu-id="6d653-237">Létrehozásával és az SSH-alagút használatával további információkért lásd: [használata SSH Tunneling Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie és egyéb web UI eléréséhez](hdinsight-linux-ambari-ssh-tunnel.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-237">For information on creating and using an SSH tunnel, see [Use SSH Tunneling to access Ambari web UI, ResourceManager, JobHistory, NameNode, Oozie, and other web UIs](hdinsight-linux-ambari-ssh-tunnel.md).</span></span>

### <a name="base-uri"></a><span data-ttu-id="6d653-238">Alap URI</span><span class="sxs-lookup"><span data-stu-id="6d653-238">Base URI</span></span>

<span data-ttu-id="6d653-239">Az alap URI-Azonosítójának a Linux-alapú HDInsight-fürtök REST API érhető el, az átjárócsomópont **https://HEADNODEFQDN:8744/api/v1/**.</span><span class="sxs-lookup"><span data-stu-id="6d653-239">The base URI for the REST API on Linux-based HDInsight clusters is available on the head node at **https://HEADNODEFQDN:8744/api/v1/**.</span></span> <span data-ttu-id="6d653-240">A tartomány nevét az átjárócsomópont jön létre a fürt létrehozása során, és nincs statikus.</span><span class="sxs-lookup"><span data-stu-id="6d653-240">The domain name of the head node is generated during cluster creation and is not static.</span></span>

<span data-ttu-id="6d653-241">A teljesen minősített tartománynevét (FQDN) találhat az átjárócsomóponthoz meg több különböző módon:</span><span class="sxs-lookup"><span data-stu-id="6d653-241">You can find the fully qualified domain name (FQDN) for the cluster head node in several different ways:</span></span>

* <span data-ttu-id="6d653-242">**Az SSH-munkamenetet**: a parancs `headnode -f` az SSH-munkamenetet a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6d653-242">**From an SSH session**: Use the command `headnode -f` from an SSH session to the cluster.</span></span>
* <span data-ttu-id="6d653-243">**Az Ambari webes**: válasszon **szolgáltatások** a lap tetején, majd válassza ki **Storm**.</span><span class="sxs-lookup"><span data-stu-id="6d653-243">**From Ambari Web**: Select **Services** from the top of the page, then select **Storm**.</span></span> <span data-ttu-id="6d653-244">Az a **összegzés** lapon jelölje be **Storm felhasználói felület Server**.</span><span class="sxs-lookup"><span data-stu-id="6d653-244">From the **Summary** tab, select **Storm UI Server**.</span></span> <span data-ttu-id="6d653-245">A Storm felhasználói felület és a REST API-t futtató csomópont teljes Tartományneve van, az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="6d653-245">The FQDN of the node that the Storm UI and REST API is running is at the top of the page.</span></span>
* <span data-ttu-id="6d653-246">**Az Ambari REST API**: a parancs `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` a csomópontot, a Storm felhasználói felület és a REST API-n futó vonatkozó információk lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="6d653-246">**From Ambari REST API**: Use the command `curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/STORM/components/STORM_UI_SERVER"` to retrieve information about the node that the Storm UI and REST API are running on.</span></span> <span data-ttu-id="6d653-247">Cserélje le **jelszó** a fürt rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="6d653-247">Replace **PASSWORD** with the admin password for the cluster.</span></span> <span data-ttu-id="6d653-248">Cserélje le **CLUSTERNAME** a fürt nevéhez.</span><span class="sxs-lookup"><span data-stu-id="6d653-248">Replace **CLUSTERNAME** with the cluster name.</span></span> <span data-ttu-id="6d653-249">A válaszban a "gazdaszámítógép_neve"-bejegyzése tartalmazza a csomópont teljesen minősített Tartománynevét.</span><span class="sxs-lookup"><span data-stu-id="6d653-249">In the response, the "host_name" entry contains the FQDN of the node.</span></span>

### <a name="authentication"></a><span data-ttu-id="6d653-250">Authentication</span><span class="sxs-lookup"><span data-stu-id="6d653-250">Authentication</span></span>

<span data-ttu-id="6d653-251">A REST API-kérésnek kell használnia **az egyszerű hitelesítés**, így a HDInsight fürt rendszergazdája nevet és jelszót használják.</span><span class="sxs-lookup"><span data-stu-id="6d653-251">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="6d653-252">Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** HTTPS protokoll használatával biztonságos kommunikáció a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6d653-252">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="6d653-253">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="6d653-253">Return values</span></span>

<span data-ttu-id="6d653-254">Információ a REST API visszaadott csak lehet a fürt vagy a virtuális gépek azonos Azure virtuális hálózaton a fürt belül használható.</span><span class="sxs-lookup"><span data-stu-id="6d653-254">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="6d653-255">Például visszaadott Zookeeper-kiszolgáló teljesen minősített tartománynevét (FQDN) nem lesz elérhető az internetről.</span><span class="sxs-lookup"><span data-stu-id="6d653-255">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers is not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6d653-256">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d653-256">Next Steps</span></span>

<span data-ttu-id="6d653-257">Most, hogy megismerte segítségével telepítheti és figyelheti a topológia a Storm irányítópultjának használatával megtudhatja, hogyan lehet [Maven használatával fejlesztése Java-alapú topológiák](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-257">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to [Develop Java-based topologies using Maven](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="6d653-258">További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="6d653-258">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>
