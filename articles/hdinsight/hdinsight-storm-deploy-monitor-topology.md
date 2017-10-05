---
title: "Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan telepítheti, figyelheti és kezelheti a Storm irányítópultjának használata a HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 5e542072-f014-42aa-82d6-2694a76df520
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/01/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 34072574f83b51280e60e2f8766c6c5d5a33c307
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="bf8cf-104">Központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="bf8cf-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="bf8cf-105">A Storm irányítópultjának segítségével egyszerűen regisztrálhat és futtathat Apache Storm-topológiák a HDInsight-fürthöz, a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-105">The Storm Dashboard allows you to easily deploy and run Apache Storm topologies to your HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="bf8cf-106">Az Irányítópult segítségével figyelheti és kezelheti a futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-106">You can also use the dashboard to monitor and manage running topologies.</span></span> <span data-ttu-id="bf8cf-107">Visual Studio használata esetén a HDInsight Tools for Visual Studio adja meg a Visual Studio hasonló szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-107">If you use Visual Studio, the HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="bf8cf-108">A Storm irányítópultjának és a HDInsight Tools Storm funkciók segítségével hozza létre a sajátját figyelés alatt futó Storm REST API és kezelési megoldásai támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-108">The Storm Dashboard and the Storm features in the HDInsight Tools rely on the Storm REST API, which can be used to create your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bf8cf-109">A jelen dokumentumban leírt lépések egy Storm on HDInsight-fürt által használt Windows operációs rendszer szükséges.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-109">The steps in this document require a Storm on HDInsight cluster that uses Windows as the operating system.</span></span> <span data-ttu-id="bf8cf-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="bf8cf-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="bf8cf-112">Telepítésével és a HDInsight-fürtök által használt Linux Storm-topológiák kezelésével kapcsolatos információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="bf8cf-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf8cf-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bf8cf-113">Prerequisites</span></span>

* <span data-ttu-id="bf8cf-114">**A HDInsight alatt futó Apache Storm** -lásd [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started.md) fürt létrehozásának lépései.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="bf8cf-115">Az a **a Storm irányítópultja**: HTML5-támogatással rendelkező modern webböngésző.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-115">For the **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="bf8cf-116">A **Visual Studio** -2.5.1-es verzióval készült Azure SDK újabb vagy és a HDInsight Tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and the HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="bf8cf-117">Lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) telepítése és konfigurálása a HDInsight tools for Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) to install and configure the HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="bf8cf-118">A Visual Studio következő verzióinak valamelyike:</span><span class="sxs-lookup"><span data-stu-id="bf8cf-118">One of the following versions of Visual Studio:</span></span>

  * <span data-ttu-id="bf8cf-119">A Visual Studio 2012 Update 4</span><span class="sxs-lookup"><span data-stu-id="bf8cf-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="bf8cf-120">Visual Studio 2013 4. frissítéssel vagy Visual Studio 2013 Community</span><span class="sxs-lookup"><span data-stu-id="bf8cf-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="bf8cf-121">A Visual Studio 2015 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="bf8cf-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="bf8cf-122">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="bf8cf-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="bf8cf-123">A Storm irányítópultja</span><span class="sxs-lookup"><span data-stu-id="bf8cf-123">Storm Dashboard</span></span>

<span data-ttu-id="bf8cf-124">A Storm irányítópultja a Storm-fürt elérhető weblapon.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-124">The Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="bf8cf-125">Az URL-címe **https://&lt;clustername >.azurehdinsight.net/**, ahol **clustername** a Storm on HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-125">The URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="bf8cf-126">Válassza ki a Storm irányítópultjának felső részén **nyújt topológia**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-126">From the top of the Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="bf8cf-127">A lapon, a minta topológia futtatásához vagy feltölteni, majd futtassa a topológia létrehozott kövesse az utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-127">Follow the instructions on the page to run a sample topology or to upload and run a topology that you created.</span></span>

![a Küldés topológia lapon][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="bf8cf-129">A Storm felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="bf8cf-129">Storm UI</span></span>

<span data-ttu-id="bf8cf-130">A Storm irányítópultjának, válassza ki a **Storm felhasználói felülete** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-130">From the Storm Dashboard, select the **Storm UI** link.</span></span> <span data-ttu-id="bf8cf-131">Ez a fürt bármely futó topológiákat mellett információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-131">This displays information about the cluster, in addition to any running topologies.</span></span>

![a storm felhasználói felülete][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="bf8cf-133">Az Internet Explorer egyes verziói azt tapasztalhatja, hogy a Storm felhasználói felülete nem frissíti először ellátogatott után.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-133">With some versions of Internet Explorer, you may discover that the Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="bf8cf-134">Például akkor lehet, hogy jelenjen meg az új topológiák meg, vagy azt előfordulhat, hogy a topológia aktívnak amikor korábban inaktív.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-134">For example, it may not show the new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="bf8cf-135">Microsoft ismeri a problémát, és a megoldás működik.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="bf8cf-136">Főoldala</span><span class="sxs-lookup"><span data-stu-id="bf8cf-136">Main page</span></span>

<span data-ttu-id="bf8cf-137">A Storm felhasználói felület a fő lapján az alábbi információkat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="bf8cf-137">The main page of the Storm UI provides the following information:</span></span>

* <span data-ttu-id="bf8cf-138">**A fürt összefoglaló**: a Storm-fürt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-138">**Cluster summary**: Basic information about the Storm cluster.</span></span>

* <span data-ttu-id="bf8cf-139">**Összegző topológia**: futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="bf8cf-140">Ebben a szakaszban a hivatkozások segítségével további információkat szeretne az adott topológiákat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-140">Use the links in this section to view more information about specific topologies.</span></span>

* <span data-ttu-id="bf8cf-141">**Összegző felügyelő**: a Storm felügyelő kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-141">**Supervisor summary**: Information about the Storm supervisor.</span></span>

* <span data-ttu-id="bf8cf-142">**Nimbus konfigurációs**: Nimbus konfigurációját a fürtben.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-142">**Nimbus configuration**: Nimbus configuration for the cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="bf8cf-143">Topológia összegzése</span><span class="sxs-lookup"><span data-stu-id="bf8cf-143">Topology summary</span></span>

<span data-ttu-id="bf8cf-144">A hivatkozás kiválasztása a **topológia összegzése** csoportban megjelennek a topológia a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="bf8cf-144">Selecting a link from the **Topology summary** section displays the following information about the topology:</span></span>

* <span data-ttu-id="bf8cf-145">**Összegző topológia**: a topológia alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-145">**Topology summary**: Basic information about the topology.</span></span>

* <span data-ttu-id="bf8cf-146">**Topológia műveletek**: kezelési műveleteket hajthat végre a topológia.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-146">**Topology actions**: Management actions that you can perform for the topology.</span></span>

  * <span data-ttu-id="bf8cf-147">**Aktiválása**: folytatása inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="bf8cf-148">**Inaktiválása**: megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="bf8cf-149">**Visszaegyensúlyozás**: Beállítja a topológia párhuzamosságát.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-149">**Rebalance**: Adjusts the parallelism of the topology.</span></span> <span data-ttu-id="bf8cf-150">A fürtben található csomópontok számának megváltoztatását követően újra ki kell egyensúlyozni a futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-150">You should rebalance running topologies after you have changed the number of nodes in the cluster.</span></span> <span data-ttu-id="bf8cf-151">Ez lehetővé teszi a topológia párhuzamosságának kiegyensúlyozása érdekében a növelő vagy csökkentő a fürtben található csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-151">This allows the topology to adjust parallelism to compensate for the increased or decreased number of nodes in the cluster.</span></span>

      <span data-ttu-id="bf8cf-152">További információkért lásd: [ismertetése a Storm-topológia (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) párhuzamosságát](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-152">For more information, see [Understanding the parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="bf8cf-153">**Kill**: a Storm-topológia leállítása bizonyos időtúllépést követően.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-153">**Kill**: Terminates a Storm topology after the specified timeout.</span></span>

* <span data-ttu-id="bf8cf-154">**Topológiastatisztikák**: a topológia statisztikája.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-154">**Topology stats**: Statistics about the topology.</span></span> <span data-ttu-id="bf8cf-155">A hivatkozások a **ablak** oszlop időkeretének megadására a többieknek beállításához a lap.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-155">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="bf8cf-156">**Spoutok**: A spoutokkal kapcsolatban, használja a topológiát.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-156">**Spouts**: The spouts used by the topology.</span></span> <span data-ttu-id="bf8cf-157">Ebben a szakaszban a hivatkozások segítségével megtekintheti az adott spoutokkal kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-157">Use the links in this section to view more information about specific spouts.</span></span>

* <span data-ttu-id="bf8cf-158">**Boltok**: A boltokhoz a topológia használják.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-158">**Bolts**: The bolts used by the topology.</span></span> <span data-ttu-id="bf8cf-159">Ebben a szakaszban található hivatkozások segítségével további információkat szeretne az adott boltokhoz.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-159">Use the links in this section to view more information about specific bolts.</span></span>

* <span data-ttu-id="bf8cf-160">**Topológiakonfiguráció**: a kiválasztott topológia beállításától.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-160">**Topology configuration**: The configuration of the selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="bf8cf-161">Spout és Bolt összegzése</span><span class="sxs-lookup"><span data-stu-id="bf8cf-161">Spout and Bolt summary</span></span>

<span data-ttu-id="bf8cf-162">Az egy spout kiválasztása a **Spoutok** vagy **boltok** szakaszok megjeleníti a kijelölt elem a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="bf8cf-162">Selecting a spout from the **Spouts** or **Bolts** sections displays the following information about the selected item:</span></span>

* <span data-ttu-id="bf8cf-163">**Az összetevő összegzés**: a spout vagy bolt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-163">**Component summary**: Basic information about the spout or bolt.</span></span>

* <span data-ttu-id="bf8cf-164">**Spout vagy Bolt statisztikák**: a spout vagy bolt statisztikája.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-164">**Spout/Bolt stats**: Statistics about the spout or bolt.</span></span> <span data-ttu-id="bf8cf-165">A hivatkozások a **ablak** oszlop időkeretének megadására a többieknek beállításához a lap.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-165">Use the links in the **Window** column to set the timeframe for the remaining entries on the page.</span></span>

* <span data-ttu-id="bf8cf-166">**Beviteli statisztikák** (csak boltok esetében): a bemeneti adatfolyamot tartalmaz a bolt által felhasznált információk.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-166">**Input stats** (bolt only): Information about the input streams consumed by the bolt.</span></span>

* <span data-ttu-id="bf8cf-167">**Kimeneti statisztikák**: az adatfolyamokat, ez által kibocsátott információ spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-167">**Output stats**: Information about the streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="bf8cf-168">**Végrehajtója**: a spout vagy bolt példányaira vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-168">**Executors**: Information about the instances of the spout or bolt.</span></span> <span data-ttu-id="bf8cf-169">Válassza ki a **Port** egy adott végrehajtó diagnosztikai adatok megtekintését, a bejegyzés erre a példányra.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-169">Select the **Port** entry for a specific executor to view a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="bf8cf-170">**Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="bf8cf-171">HDInsight Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="bf8cf-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="bf8cf-172">A HDInsight Tools elküldeni a C# vagy hibrid topológiák a Storm fürthöz használható.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-172">The HDInsight Tools can be used to submit C# or hybrid topologies to your Storm cluster.</span></span> <span data-ttu-id="bf8cf-173">Az alábbi lépéseket a mintaalkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-173">The following steps use a sample application.</span></span> <span data-ttu-id="bf8cf-174">A HDInsight Tools használatával saját topológiák létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák a HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-174">For information about creating your own topologies by using the HDInsight Tools, see [Develop C# topologies using the HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="bf8cf-175">Az alábbi lépések segítségével telepít egy alatt futó Storm on HDInsight-fürt, akkor megtekintheti, és a topológia kezelése.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-175">Use the following steps to deploy a sample to your Storm on HDInsight cluster, then view and manage the topology.</span></span>

1. <span data-ttu-id="bf8cf-176">Ha még nem telepítette a legújabb verzióját a HDInsight Tools for Visual Studio, lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-176">If you have not already installed the latest version of the HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="bf8cf-177">Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="bf8cf-178">Az a **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-178">In the **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="bf8cf-179">Válassza ki a listáról a sablonok **Storm minta**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-179">From the list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="bf8cf-180">A párbeszédpanel alján írja be az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-180">At the bottom of the dialog box, type a name for the application.</span></span>

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="bf8cf-182">A **Megoldáskezelőben**, kattintson jobb gombbal a projektre, és válassza ki **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-182">In **Solution Explorer**, right-click the project, and select **Submit to Storm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bf8cf-183">Ha a rendszer kéri, adja meg a bejelentkezési hitelesítő adatok az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-183">If prompted, enter the login credentials for your Azure subscription.</span></span> <span data-ttu-id="bf8cf-184">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be, amely tartalmazza a Storm on HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-184">If you have more than one subscription, log in to the one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="bf8cf-185">Válassza ki a Storm on HDInsight-fürt a a **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-185">Select your Storm on HDInsight cluster from the **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="bf8cf-186">-E a küldése sikeres használatával figyelheti a **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-186">You can monitor whether the submission is successful by using the **Output** window.</span></span>

6. <span data-ttu-id="bf8cf-187">Ha a topológia küldése sikeres volt, a **Storm-topológiák** megjelenjen-e a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-187">When the topology has been successfully submitted, the **Storm Topologies** for the cluster should appear.</span></span> <span data-ttu-id="bf8cf-188">Válassza ki a topológiát a futó topológiát vonatkozó információk megtekintése a listából.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-188">Select the topology from the list to view information about the running topology.</span></span>

    ![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="bf8cf-190">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="bf8cf-191">Válassza ki az alakzat a spoutokkal kapcsolatban, vagy a boltokhoz ezeket az összetevőket vonatkozó információk megtekintése.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-191">Select the shape for the spouts or bolts to view information about these components.</span></span> <span data-ttu-id="bf8cf-192">Minden elem kijelölve egy új ablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bf8cf-193">A topológia neve nem az osztály nevét a topológia (ebben az esetben `HelloWord`,) az időbélyegzőnek lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-193">The name of the topology is the class name of the topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="bf8cf-194">Az a **topológia összegzése** nézetben jelölje ki **Kill** a topológia leállítása.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-194">From the **Topology Summary** view, select **Kill** to stop the topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="bf8cf-195">Storm-topológiák továbbra is fut le vannak állítva, vagy a fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-195">Storm topologies continue running until they are stopped or the cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="bf8cf-196">REST API</span><span class="sxs-lookup"><span data-stu-id="bf8cf-196">REST API</span></span>

<span data-ttu-id="bf8cf-197">A Storm felhasználói felülete a REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, a REST API használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-197">The Storm UI is built on top of the REST API, so you can perform similar management and monitoring functionality by using the REST API.</span></span> <span data-ttu-id="bf8cf-198">A REST API-t hozhat létre egyéni eszközök felügyelheti és figyelheti a Storm-topológiák.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-198">You can use the REST API to create custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="bf8cf-199">További információkért lásd: [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="bf8cf-200">Az alábbi információk csak a HDInsight alatt futó Apache Storm a REST API használatával az elő.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-200">The following information is specific to using the REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="bf8cf-201">Alap URI</span><span class="sxs-lookup"><span data-stu-id="bf8cf-201">Base URI</span></span>

<span data-ttu-id="bf8cf-202">Az alap URI a HDInsight-fürtök REST API **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, ahol **clustername** a Storm on HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-202">The base URI for the REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is the name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="bf8cf-203">Authentication</span><span class="sxs-lookup"><span data-stu-id="bf8cf-203">Authentication</span></span>

<span data-ttu-id="bf8cf-204">A REST API-kérésnek kell használnia **az egyszerű hitelesítés**, így a HDInsight fürt rendszergazdája nevet és jelszót használják.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-204">Requests to the REST API must use **basic authentication**, so you use the HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="bf8cf-205">Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** HTTPS protokoll használatával biztonságos kommunikáció a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS to secure communications with the cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="bf8cf-206">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="bf8cf-206">Return values</span></span>

<span data-ttu-id="bf8cf-207">Információ a REST API visszaadott csak lehet a fürt vagy a virtuális gépek azonos Azure virtuális hálózaton a fürt belül használható.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-207">Information that is returned from the REST API may only be usable from within the cluster or virtual machines on the same Azure Virtual Network as the cluster.</span></span> <span data-ttu-id="bf8cf-208">Például a teljesen minősített tartománynevét (FQDN) adott vissza a rendszer a Zookeeper kiszolgálók nem lehet elérhető az internetről.</span><span class="sxs-lookup"><span data-stu-id="bf8cf-208">For example, the fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from the Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf8cf-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf8cf-209">Next Steps</span></span>

<span data-ttu-id="bf8cf-210">Most, hogy megismerte segítségével telepítheti és figyelheti a topológia a Storm irányítópultjának használatával megtudhatja, hogyan lehet:</span><span class="sxs-lookup"><span data-stu-id="bf8cf-210">Now that you've learned how to deploy and monitor topologies by using the Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="bf8cf-211">Fejlesztésére C#-topológiák a HDInsight Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="bf8cf-211">Develop C# topologies using the HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="bf8cf-212">Maven használatával Java-alapú topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="bf8cf-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="bf8cf-213">További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="bf8cf-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
