---
title: "aaaDeploy és kezelése a HDInsight alatt futó Apache Storm-topológiák |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy, figyelheti és kezelheti a Storm irányítópultja hello használata a HDInsight alatt futó Apache Storm-topológiák. Visual Studio Hadoop-eszközök használata."
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
ms.openlocfilehash: 05f05fe8dd519fe99fb771d36bfc3d28168ca85f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-apache-storm-topologies-on-windows-based-hdinsight"></a><span data-ttu-id="d24fe-104">Központi telepítése és kezelése a Windows-alapú HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="d24fe-104">Deploy and manage Apache Storm topologies on Windows-based HDInsight</span></span>

<span data-ttu-id="d24fe-105">a Storm irányítópultja hello lehetővé teszi a tooeasily telepítésére és alatt futó Apache Storm topológiák tooyour HDInsight-fürt futtatására a webböngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="d24fe-105">hello Storm Dashboard allows you tooeasily deploy and run Apache Storm topologies tooyour HDInsight cluster by using your web browser.</span></span> <span data-ttu-id="d24fe-106">Is hello irányítópult toomonitor használja, és kezelheti a futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-106">You can also use hello dashboard toomonitor and manage running topologies.</span></span> <span data-ttu-id="d24fe-107">Visual Studio használata, ha a hello a HDInsight Tools for Visual Studio tartalmaz a Visual Studio hasonló funkciókat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-107">If you use Visual Studio, hello HDInsight Tools for Visual Studio provide similar features in Visual Studio.</span></span>

<span data-ttu-id="d24fe-108">a Storm irányítópultja hello és hello Storm szolgáltatások hello a HDInsight Tools támaszkodjon hello Storm REST API, amely lehet használt toocreate saját figyelése és felügyeleti megoldásokat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-108">hello Storm Dashboard and hello Storm features in hello HDInsight Tools rely on hello Storm REST API, which can be used toocreate your own monitoring and management solutions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d24fe-109">hello jelen dokumentumban leírt lépések egy Storm on HDInsight-fürt által használt Windows hello operációs rendszert igényelnek.</span><span class="sxs-lookup"><span data-stu-id="d24fe-109">hello steps in this document require a Storm on HDInsight cluster that uses Windows as hello operating system.</span></span> <span data-ttu-id="d24fe-110">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="d24fe-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="d24fe-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="d24fe-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>
>
> <span data-ttu-id="d24fe-112">Telepítésével és a HDInsight-fürtök által használt Linux Storm-topológiák kezelésével kapcsolatos információkért lásd: [központi telepítése és kezelése a Linux-alapú HDInsight alatt futó Apache Storm-topológiák](hdinsight-storm-deploy-monitor-topology-linux.md)</span><span class="sxs-lookup"><span data-stu-id="d24fe-112">For information on deploying and managing Storm topologies with an HDInsight cluster that uses Linux, see [Deploy and manage Apache Storm topologies on Linux-based HDInsight](hdinsight-storm-deploy-monitor-topology-linux.md)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d24fe-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d24fe-113">Prerequisites</span></span>

* <span data-ttu-id="d24fe-114">**A HDInsight alatt futó Apache Storm** -lásd [első lépései a HDInsight alatt futó Apache Storm](hdinsight-apache-storm-tutorial-get-started.md) fürt létrehozásának lépései.</span><span class="sxs-lookup"><span data-stu-id="d24fe-114">**Apache Storm on HDInsight** - see [Get started with Apache Storm on HDInsight](hdinsight-apache-storm-tutorial-get-started.md) for steps on creating a cluster.</span></span>

* <span data-ttu-id="d24fe-115">A hello **a Storm irányítópultja**: HTML5-támogatással rendelkező modern webböngésző.</span><span class="sxs-lookup"><span data-stu-id="d24fe-115">For hello **Storm Dashboard**: A modern web browser that supports HTML5.</span></span>

* <span data-ttu-id="d24fe-116">A **Visual Studio** -2.5.1-es verzióval készült Azure SDK újabb vagy és a HDInsight Tools for Visual Studio hello.</span><span class="sxs-lookup"><span data-stu-id="d24fe-116">For **Visual Studio** - Azure SDK 2.5.1 or newer and hello HDInsight Tools for Visual Studio.</span></span> <span data-ttu-id="d24fe-117">Lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall és hello a HDInsight tools for Visual Studio konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d24fe-117">See [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md) tooinstall and configure hello HDInsight tools for Visual Studio.</span></span>

    <span data-ttu-id="d24fe-118">A következő Visual Studio verziójának hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="d24fe-118">One of hello following versions of Visual Studio:</span></span>

  * <span data-ttu-id="d24fe-119">A Visual Studio 2012 Update 4</span><span class="sxs-lookup"><span data-stu-id="d24fe-119">Visual Studio 2012 with Update 4</span></span>

  * <span data-ttu-id="d24fe-120">Visual Studio 2013 4. frissítéssel vagy Visual Studio 2013 Community</span><span class="sxs-lookup"><span data-stu-id="d24fe-120">Visual Studio 2013 with Update 4 or Visual Studio 2013 Community</span></span>

  * <span data-ttu-id="d24fe-121">A Visual Studio 2015 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="d24fe-121">Visual Studio 2015 (any edition)</span></span>

  * <span data-ttu-id="d24fe-122">A Visual Studio 2017 (minden kiadás)</span><span class="sxs-lookup"><span data-stu-id="d24fe-122">Visual Studio 2017 (any edition)</span></span>

## <a name="storm-dashboard"></a><span data-ttu-id="d24fe-123">A Storm irányítópultja</span><span class="sxs-lookup"><span data-stu-id="d24fe-123">Storm Dashboard</span></span>

<span data-ttu-id="d24fe-124">a Storm irányítópultja hello kerül, a Storm-fürt érhető el.</span><span class="sxs-lookup"><span data-stu-id="d24fe-124">hello Storm Dashboard is a web page available on your Storm cluster.</span></span> <span data-ttu-id="d24fe-125">hello URL-cím **https://&lt;clustername >.azurehdinsight.net/**, ahol **clustername** hello a Storm on HDInsight-fürt neve.</span><span class="sxs-lookup"><span data-stu-id="d24fe-125">hello URL is **https://&lt;clustername>.azurehdinsight.net/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

<span data-ttu-id="d24fe-126">Válassza ki a Storm irányítópultja hello hello felső részén **nyújt topológia**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-126">From hello top of hello Storm Dashboard, select **Submit Topology**.</span></span> <span data-ttu-id="d24fe-127">Kövesse az utasításokat hello hello lap toorun egy minta topológia vagy tooupload, és futtassa a topológia létrehozott.</span><span class="sxs-lookup"><span data-stu-id="d24fe-127">Follow hello instructions on hello page toorun a sample topology or tooupload and run a topology that you created.</span></span>

![hello küldje el a topológia lapon][storm-dashboard-submit]

### <a name="storm-ui"></a><span data-ttu-id="d24fe-129">A Storm felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="d24fe-129">Storm UI</span></span>

<span data-ttu-id="d24fe-130">A Storm irányítópultja hello, válassza ki hello **Storm felhasználói felülete** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="d24fe-130">From hello Storm Dashboard, select hello **Storm UI** link.</span></span> <span data-ttu-id="d24fe-131">A futó topológiákkal hozzáadása tooany hello fürt, információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="d24fe-131">This displays information about hello cluster, in addition tooany running topologies.</span></span>

![hello storm felhasználói felülete][storm-dashboard-ui]

> [!NOTE]
> <span data-ttu-id="d24fe-133">Internet Explorer egyes verziói azt tapasztalhatja, hogy hello Storm felhasználói felülete követően először ellátogatott nem frissül.</span><span class="sxs-lookup"><span data-stu-id="d24fe-133">With some versions of Internet Explorer, you may discover that hello Storm UI does not refresh after you have first visited it.</span></span> <span data-ttu-id="d24fe-134">Például akkor lehet, hogy megjelenni hello új topológiák meg, vagy azt előfordulhat, hogy a topológia aktívnak amikor korábban inaktív.</span><span class="sxs-lookup"><span data-stu-id="d24fe-134">For example, it may not show hello new topologies you submitted, or it may show a topology as active when you previously deactivated it.</span></span> <span data-ttu-id="d24fe-135">Microsoft ismeri a problémát, és a megoldás működik.</span><span class="sxs-lookup"><span data-stu-id="d24fe-135">Microsoft is aware of this issue and is working on a solution.</span></span>

#### <a name="main-page"></a><span data-ttu-id="d24fe-136">Főoldala</span><span class="sxs-lookup"><span data-stu-id="d24fe-136">Main page</span></span>

<span data-ttu-id="d24fe-137">hello fő lapján hello Storm felhasználói felülete hello a következő információkat biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d24fe-137">hello main page of hello Storm UI provides hello following information:</span></span>

* <span data-ttu-id="d24fe-138">**A fürt összefoglaló**: hello Storm-fürt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="d24fe-138">**Cluster summary**: Basic information about hello Storm cluster.</span></span>

* <span data-ttu-id="d24fe-139">**Összegző topológia**: futó topológiákat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-139">**Topology summary**: A list of running topologies.</span></span> <span data-ttu-id="d24fe-140">Ez a szakasz tooview a hello hivatkozások használata az további információt adott topológiák kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="d24fe-140">Use hello links in this section tooview more information about specific topologies.</span></span>

* <span data-ttu-id="d24fe-141">**Felügyelő összefoglaló**: hello Storm felügyelő kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-141">**Supervisor summary**: Information about hello Storm supervisor.</span></span>

* <span data-ttu-id="d24fe-142">**Nimbus konfigurációs**: hello fürt Nimbus konfigurációjában.</span><span class="sxs-lookup"><span data-stu-id="d24fe-142">**Nimbus configuration**: Nimbus configuration for hello cluster.</span></span>

#### <a name="topology-summary"></a><span data-ttu-id="d24fe-143">Topológia összegzése</span><span class="sxs-lookup"><span data-stu-id="d24fe-143">Topology summary</span></span>

<span data-ttu-id="d24fe-144">Egy hivatkozási kiválasztása a hello **topológia összegzése** szakasz hello topológiára vonatkozó adatokat a következő hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d24fe-144">Selecting a link from hello **Topology summary** section displays hello following information about hello topology:</span></span>

* <span data-ttu-id="d24fe-145">**Összegző topológia**: hello topológia alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="d24fe-145">**Topology summary**: Basic information about hello topology.</span></span>

* <span data-ttu-id="d24fe-146">**Topológia műveletek**: hello topológia végrehajtható felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="d24fe-146">**Topology actions**: Management actions that you can perform for hello topology.</span></span>

  * <span data-ttu-id="d24fe-147">**Aktiválása**: folytatása inaktivált topológia feldolgozását.</span><span class="sxs-lookup"><span data-stu-id="d24fe-147">**Activate**: Resumes processing of a deactivated topology.</span></span>

  * <span data-ttu-id="d24fe-148">**Inaktiválása**: megszakítja a futó topológiát.</span><span class="sxs-lookup"><span data-stu-id="d24fe-148">**Deactivate**: Pauses a running topology.</span></span>

  * <span data-ttu-id="d24fe-149">**Visszaegyensúlyozás**: hello topológia párhuzamosságát hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d24fe-149">**Rebalance**: Adjusts hello parallelism of hello topology.</span></span> <span data-ttu-id="d24fe-150">Futó topológiákat kell egyensúlyba, miután módosította hello fürtben található csomópontok számának hello.</span><span class="sxs-lookup"><span data-stu-id="d24fe-150">You should rebalance running topologies after you have changed hello number of nodes in hello cluster.</span></span> <span data-ttu-id="d24fe-151">Ez lehetővé teszi, hogy hello topológia tooadjust párhuzamossági toocompensate hello növelhető vagy csökkenthető, hello fürtben található csomópontok számát.</span><span class="sxs-lookup"><span data-stu-id="d24fe-151">This allows hello topology tooadjust parallelism toocompensate for hello increased or decreased number of nodes in hello cluster.</span></span>

      <span data-ttu-id="d24fe-152">További információkért lásd: [ismertetése a Storm-topológia (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) hello párhuzamosságát](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span><span class="sxs-lookup"><span data-stu-id="d24fe-152">For more information, see [Understanding hello parallelism of a Storm topology (http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html)](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html).</span></span>

  * <span data-ttu-id="d24fe-153">**Kill**: a Storm-topológia leállítása után hello megadott időkorlát.</span><span class="sxs-lookup"><span data-stu-id="d24fe-153">**Kill**: Terminates a Storm topology after hello specified timeout.</span></span>

* <span data-ttu-id="d24fe-154">**Topológiastatisztikák**: hello topológia statisztikája.</span><span class="sxs-lookup"><span data-stu-id="d24fe-154">**Topology stats**: Statistics about hello topology.</span></span> <span data-ttu-id="d24fe-155">Hello hello hivatkozásokkal **ablak** oszlop tooset hello időkeretre hello fennmaradó bejegyzések hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="d24fe-155">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="d24fe-156">**Spoutok**: hello hello topológia által használt spoutokkal kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d24fe-156">**Spouts**: hello spouts used by hello topology.</span></span> <span data-ttu-id="d24fe-157">Ez a szakasz tooview a hello hivatkozások használata az adott spoutokkal kapcsolatban további információt.</span><span class="sxs-lookup"><span data-stu-id="d24fe-157">Use hello links in this section tooview more information about specific spouts.</span></span>

* <span data-ttu-id="d24fe-158">**Boltok**: hello boltokhoz hello topológia használják.</span><span class="sxs-lookup"><span data-stu-id="d24fe-158">**Bolts**: hello bolts used by hello topology.</span></span> <span data-ttu-id="d24fe-159">Ez a szakasz tooview a hello hivatkozások használata az adott boltokhoz további információt.</span><span class="sxs-lookup"><span data-stu-id="d24fe-159">Use hello links in this section tooview more information about specific bolts.</span></span>

* <span data-ttu-id="d24fe-160">**Topológiakonfiguráció**: hello hello kiválasztott topológia beállításától.</span><span class="sxs-lookup"><span data-stu-id="d24fe-160">**Topology configuration**: hello configuration of hello selected topology.</span></span>

#### <a name="spout-and-bolt-summary"></a><span data-ttu-id="d24fe-161">Spout és Bolt összegzése</span><span class="sxs-lookup"><span data-stu-id="d24fe-161">Spout and Bolt summary</span></span>

<span data-ttu-id="d24fe-162">Egy spout kijelölésével hello **Spoutok** vagy **boltok** szakaszok hello kijelölt elemre vonatkozó információkat a következő hello jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="d24fe-162">Selecting a spout from hello **Spouts** or **Bolts** sections displays hello following information about hello selected item:</span></span>

* <span data-ttu-id="d24fe-163">**Az összetevő összegzés**: hello spout vagy bolt alapvető adatait.</span><span class="sxs-lookup"><span data-stu-id="d24fe-163">**Component summary**: Basic information about hello spout or bolt.</span></span>

* <span data-ttu-id="d24fe-164">**Spout vagy Bolt statisztikák**: hello statisztikája spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="d24fe-164">**Spout/Bolt stats**: Statistics about hello spout or bolt.</span></span> <span data-ttu-id="d24fe-165">Hello hello hivatkozásokkal **ablak** oszlop tooset hello időkeretre hello fennmaradó bejegyzések hello oldalon.</span><span class="sxs-lookup"><span data-stu-id="d24fe-165">Use hello links in hello **Window** column tooset hello timeframe for hello remaining entries on hello page.</span></span>

* <span data-ttu-id="d24fe-166">**Beviteli statisztikák** (csak boltok esetében): hello információ bemenetét hello bolt által használt.</span><span class="sxs-lookup"><span data-stu-id="d24fe-166">**Input stats** (bolt only): Information about hello input streams consumed by hello bolt.</span></span>

* <span data-ttu-id="d24fe-167">**Kimeneti statisztikák**: hello adatfolyamok által kibocsátott információ spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="d24fe-167">**Output stats**: Information about hello streams emitted by this spout or bolt.</span></span>

* <span data-ttu-id="d24fe-168">**Végrehajtója**: hello spout vagy bolt hello példányaira vonatkozó információkat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-168">**Executors**: Information about hello instances of hello spout or bolt.</span></span> <span data-ttu-id="d24fe-169">Jelölje be hello **Port** egy adott végrehajtó tooview bejegyzést a diagnosztikai adatokat tartalmazó naplófájlt előállított erre a példányra.</span><span class="sxs-lookup"><span data-stu-id="d24fe-169">Select hello **Port** entry for a specific executor tooview a log of diagnostic information produced for this instance.</span></span>

* <span data-ttu-id="d24fe-170">**Hibák**: bármilyen hiba adatokat a spout vagy boltok esetében.</span><span class="sxs-lookup"><span data-stu-id="d24fe-170">**Errors**: Any error information for this spout or bolt.</span></span>

## <a name="hdinsight-tools-for-visual-studio"></a><span data-ttu-id="d24fe-171">HDInsight Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d24fe-171">HDInsight Tools for Visual Studio</span></span>

<span data-ttu-id="d24fe-172">a HDInsight Tools hello lehet használt toosubmit C# vagy hibrid topológiák tooyour Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="d24fe-172">hello HDInsight Tools can be used toosubmit C# or hybrid topologies tooyour Storm cluster.</span></span> <span data-ttu-id="d24fe-173">a lépéseket követve hello mintaalkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="d24fe-173">hello following steps use a sample application.</span></span> <span data-ttu-id="d24fe-174">A saját topológiák hello a HDInsight Tools használatával létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d24fe-174">For information about creating your own topologies by using hello HDInsight Tools, see [Develop C# topologies using hello HDInsight Tools for Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

<span data-ttu-id="d24fe-175">Hello követő lépéseket toodeploy egy minta tooyour Storm on HDInsight-fürt használatára, majd megtekintheti, és hello topológia kezelése.</span><span class="sxs-lookup"><span data-stu-id="d24fe-175">Use hello following steps toodeploy a sample tooyour Storm on HDInsight cluster, then view and manage hello topology.</span></span>

1. <span data-ttu-id="d24fe-176">Ha még nem telepítette a HDInsight Tools hello hello legújabb verzióját a Visual Studio, lásd: [első lépései a HDInsight Tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="d24fe-176">If you have not already installed hello latest version of hello HDInsight Tools for Visual Studio, see [Get started using HDInsight Tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="d24fe-177">Nyissa meg a Visual Studio, válassza ki **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-177">Open Visual Studio, select **File** > **New** > **Project**.</span></span>

3. <span data-ttu-id="d24fe-178">A hello **új projekt** párbeszédpanelen bontsa ki **telepített** > **sablonok**, majd válassza ki **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-178">In hello **New Project** dialog box, expand **Installed** > **Templates**, and then select **HDInsight**.</span></span> <span data-ttu-id="d24fe-179">Válassza ki a sablonok hello listáról **Storm minta**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-179">From hello list of templates, select **Storm Sample**.</span></span> <span data-ttu-id="d24fe-180">Hello hello párbeszédpanel alsó részén írja be a hello alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="d24fe-180">At hello bottom of hello dialog box, type a name for hello application.</span></span>

    ![Kép](./media/hdinsight-storm-deploy-monitor-topology/sample.png)

4. <span data-ttu-id="d24fe-182">A **Megoldáskezelőben**, kattintson a jobb gombbal a hello projektet, és válassza ki **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-182">In **Solution Explorer**, right-click hello project, and select **Submit tooStorm on HDInsight**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d24fe-183">Ha a rendszer kéri, adja meg hello bejelentkezési hitelesítő adatait az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d24fe-183">If prompted, enter hello login credentials for your Azure subscription.</span></span> <span data-ttu-id="d24fe-184">Ha egynél több előfizetéssel rendelkezik, jelentkezzen be a Storm on HDInsight-fürt tartalmazó toohello.</span><span class="sxs-lookup"><span data-stu-id="d24fe-184">If you have more than one subscription, log in toohello one that contains your Storm on HDInsight cluster.</span></span>

5. <span data-ttu-id="d24fe-185">Válassza ki a Storm on HDInsight-fürt hello **Storm-fürt** legördülő listából válassza ki, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-185">Select your Storm on HDInsight cluster from hello **Storm Cluster** drop-down list, and then select **Submit**.</span></span> <span data-ttu-id="d24fe-186">Figyelheti, hogy hello elküldése sikeres hello segítségével **kimeneti** ablak.</span><span class="sxs-lookup"><span data-stu-id="d24fe-186">You can monitor whether hello submission is successful by using hello **Output** window.</span></span>

6. <span data-ttu-id="d24fe-187">Amikor hello topológia sikeresen elküldte, hello **Storm-topológiák** a hello fürt megjelenjen-e.</span><span class="sxs-lookup"><span data-stu-id="d24fe-187">When hello topology has been successfully submitted, hello **Storm Topologies** for hello cluster should appear.</span></span> <span data-ttu-id="d24fe-188">Válassza ki a hello topológiát hello lista tooview információt a futtató topológia hello.</span><span class="sxs-lookup"><span data-stu-id="d24fe-188">Select hello topology from hello list tooview information about hello running topology.</span></span>

    ![a Visual studio-figyelő](./media/hdinsight-storm-deploy-monitor-topology/vsmonitor.png)

   > [!NOTE]
   > <span data-ttu-id="d24fe-190">Megtekintheti továbbá **Storm-topológiák** a **Server Explorer** kibontásával **Azure** > **HDInsight**, és, majd kattintson a jobb gombbal a Storm on HDInsight-fürt, majd válassza **nézet Storm-topológiák**.</span><span class="sxs-lookup"><span data-stu-id="d24fe-190">You can also view **Storm Topologies** from **Server Explorer** by expanding **Azure** > **HDInsight**, and then right-clicking a Storm on HDInsight cluster, and selecting **View Storm Topologies**.</span></span>

    <span data-ttu-id="d24fe-191">Válassza ki hello alakzat hello spoutok vagy boltok tooview információ ezeket az összetevőket.</span><span class="sxs-lookup"><span data-stu-id="d24fe-191">Select hello shape for hello spouts or bolts tooview information about these components.</span></span> <span data-ttu-id="d24fe-192">Minden elem kijelölve egy új ablakban nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="d24fe-192">A new window opens for each item selected.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d24fe-193">hello topológia hello neve nem hello topológia hello osztály neve (ebben az esetben `HelloWord`,) az időbélyegzőnek lesz hozzáfűzve.</span><span class="sxs-lookup"><span data-stu-id="d24fe-193">hello name of hello topology is hello class name of hello topology (in this case, `HelloWord`,) with a timestamp appended.</span></span>

7. <span data-ttu-id="d24fe-194">A hello **topológia összegzése** nézetben jelölje ki **Kill** toostop hello topológia.</span><span class="sxs-lookup"><span data-stu-id="d24fe-194">From hello **Topology Summary** view, select **Kill** toostop hello topology.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d24fe-195">Storm-topológiák továbbra is fut le vannak állítva, vagy hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="d24fe-195">Storm topologies continue running until they are stopped or hello cluster is deleted.</span></span>


## <a name="rest-api"></a><span data-ttu-id="d24fe-196">REST API</span><span class="sxs-lookup"><span data-stu-id="d24fe-196">REST API</span></span>

<span data-ttu-id="d24fe-197">hello Storm felhasználói felülete hello REST API-t épül, ezért hasonló felügyeleti és figyelési funkcióit, hello REST API használatával végezheti el.</span><span class="sxs-lookup"><span data-stu-id="d24fe-197">hello Storm UI is built on top of hello REST API, so you can perform similar management and monitoring functionality by using hello REST API.</span></span> <span data-ttu-id="d24fe-198">Felügyelheti és figyelheti a Storm-topológiák hello REST API toocreate egyéni eszközöket is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d24fe-198">You can use hello REST API toocreate custom tools for managing and monitoring Storm topologies.</span></span>

<span data-ttu-id="d24fe-199">További információkért lásd: [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span><span class="sxs-lookup"><span data-stu-id="d24fe-199">For more information, see [Storm UI REST API](https://github.com/apache/storm/blob/0.9.3-branch/STORM-UI-REST-API.md).</span></span> <span data-ttu-id="d24fe-200">hello következő információkat az adott toousing hello REST API-t a HDInsight alatt futó Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="d24fe-200">hello following information is specific toousing hello REST API with Apache Storm on HDInsight.</span></span>

### <a name="base-uri"></a><span data-ttu-id="d24fe-201">Alap URI</span><span class="sxs-lookup"><span data-stu-id="d24fe-201">Base URI</span></span>

<span data-ttu-id="d24fe-202">a HDInsight-fürtök REST API hello alap URI hello **https://&lt;clustername >.azurehdinsight.net/stormui/api/v1/**, ahol **clustername** hello alatt futó Storm-neve megtalálható HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d24fe-202">hello base URI for hello REST API on HDInsight clusters is **https://&lt;clustername>.azurehdinsight.net/stormui/api/v1/**, where **clustername** is hello name of your Storm on HDInsight cluster.</span></span>

### <a name="authentication"></a><span data-ttu-id="d24fe-203">Authentication</span><span class="sxs-lookup"><span data-stu-id="d24fe-203">Authentication</span></span>

<span data-ttu-id="d24fe-204">REST API-t kell használnia kérelmek toohello **az egyszerű hitelesítés**, így a hello HDInsight fürt rendszergazdája nevét és jelszavát használja.</span><span class="sxs-lookup"><span data-stu-id="d24fe-204">Requests toohello REST API must use **basic authentication**, so you use hello HDInsight cluster administrator name and password.</span></span>

> [!NOTE]
> <span data-ttu-id="d24fe-205">Egyszerű hitelesítést a rendszer a tiszta szöveges küldi el, mert akkor **mindig** hello fürt toosecure HTTPS-kommunikációhoz használni.</span><span class="sxs-lookup"><span data-stu-id="d24fe-205">Because basic authentication is sent by using clear text, you should **always** use HTTPS toosecure communications with hello cluster.</span></span>

### <a name="return-values"></a><span data-ttu-id="d24fe-206">Visszatérési érték</span><span class="sxs-lookup"><span data-stu-id="d24fe-206">Return values</span></span>

<span data-ttu-id="d24fe-207">REST API-t csak lehet hello fürtön belül használható, vagy a virtuális gépek azonos Azure Virtual Network hello fürtként hello hello visszaadott adatokat.</span><span class="sxs-lookup"><span data-stu-id="d24fe-207">Information that is returned from hello REST API may only be usable from within hello cluster or virtual machines on hello same Azure Virtual Network as hello cluster.</span></span> <span data-ttu-id="d24fe-208">Például hello teljesen minősített tartománynevét (FQDN) érkezett Zookeeper kiszolgálók a rendszer nem érhető el hello Internet.</span><span class="sxs-lookup"><span data-stu-id="d24fe-208">For example, hello fully qualified domain name (FQDN) returned for Zookeeper servers are not be accessible from hello Internet.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d24fe-209">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d24fe-209">Next Steps</span></span>

<span data-ttu-id="d24fe-210">Most, hogy megismerte a hogyan toodeploy és a figyelő topológiák használatával hello a Storm irányítópultja, megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="d24fe-210">Now that you've learned how toodeploy and monitor topologies by using hello Storm Dashboard, learn how to:</span></span>

* [<span data-ttu-id="d24fe-211">Fejlesztésére C#-topológiák hello HDInsight Tools for Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="d24fe-211">Develop C# topologies using hello HDInsight Tools for Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

* [<span data-ttu-id="d24fe-212">Maven használatával Java-alapú topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d24fe-212">Develop Java-based topologies using Maven</span></span>](hdinsight-storm-develop-java-topology.md)

<span data-ttu-id="d24fe-213">További példa topológiák listájáért lásd: [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="d24fe-213">For a list of more example topologies, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

[hdinsight-dashboard]: ./media/hdinsight-storm-deploy-monitor-topology/dashboard-link.png
[storm-dashboard-submit]: ./media/hdinsight-storm-deploy-monitor-topology/submit.png
[storm-dashboard-ui]: ./media/hdinsight-storm-deploy-monitor-topology/storm-ui-summary.png
