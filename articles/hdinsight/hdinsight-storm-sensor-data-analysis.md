---
title: "az Apache Storm és HBase aaaAnalyze érzékelőadatait |} Microsoft Docs"
description: "Megtudhatja, hogyan tooconnect tooApache Storm egy virtuális hálózattal. A HBase tooprocess érzékelőadatait az eseményközpontban lévő Storm használjon, és jelenítheti meg azt a D3.js."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a9a1ac8e-5708-4833-b965-e453815e671f
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/09/2017
ms.author: larryfr
ms.openlocfilehash: e54fe9ffc720b0089f90e302b24a9438bd43999a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a><span data-ttu-id="0a0cd-104">Apache Storm, az Event Hubs és a HBase a HDInsight (Hadoop) érzékelőadatok elemzése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-104">Analyze sensor data with Apache Storm, Event Hub, and HBase in HDInsight (Hadoop)</span></span>

<span data-ttu-id="0a0cd-105">Megtudhatja, hogyan toouse Apache Storm a HDInsight tooprocess az Azure Event Hubs érzékelőadatait.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-105">Learn how toouse Apache Storm on HDInsight tooprocess sensor data from Azure Event Hub.</span></span> <span data-ttu-id="0a0cd-106">hello adatok az Apache HBase a HDInsight majd tárolja, és ábrázolt D3.js használatával.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-106">hello data is then stored into Apache HBase on HDInsight, and visualized using D3.js.</span></span>

<span data-ttu-id="0a0cd-107">Ebben a dokumentumban használt hello Azure Resource Manager sablon bemutatja, hogyan toocreate több Azure-erőforrások erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-107">hello Azure Resource Manager template used in this document demonstrates how toocreate multiple Azure resources in a resource group.</span></span> <span data-ttu-id="0a0cd-108">hello sablont hoz létre egy Azure virtuális hálózatra, két HDInsight-fürtök (a Storm és HBase) és az Azure Web Apps.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-108">hello template creates an Azure Virtual Network, two HDInsight clusters (Storm and HBase) and an Azure Web App.</span></span> <span data-ttu-id="0a0cd-109">A valós idejű webes irányítópult node.js végrehajtásának automatikusan telepített toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-109">A node.js implementation of a real-time web dashboard is automatically deployed toohello web app.</span></span>

> [!NOTE]
> <span data-ttu-id="0a0cd-110">a dokumentum és a jelen dokumentum példa hello adatokat HDInsight 3.6 verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-110">hello information in this document and example in this document require HDInsight version 3.6.</span></span>
>
> <span data-ttu-id="0a0cd-111">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0a0cd-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a0cd-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a0cd-113">Prerequisites</span></span>

* <span data-ttu-id="0a0cd-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-114">An Azure subscription.</span></span>
* <span data-ttu-id="0a0cd-115">[NODE.js](http://nodejs.org/): használt toopreview hello webes irányítópult helyileg a fejlesztési környezetet az.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-115">[Node.js](http://nodejs.org/): Used toopreview hello web dashboard locally on your development environment.</span></span>
* <span data-ttu-id="0a0cd-116">[Java és hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): toodevelop hello Storm-topológia használt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-116">[Java and hello JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Used toodevelop hello Storm topology.</span></span>
* <span data-ttu-id="0a0cd-117">[Maven](http://maven.apache.org/what-is-maven.html): használt toobuild és fordítási hello projekt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-117">[Maven](http://maven.apache.org/what-is-maven.html): Used toobuild and compile hello project.</span></span>
* <span data-ttu-id="0a0cd-118">[Git](http://git-scm.com/): használt toodownload hello projektet a Githubról.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-118">[Git](http://git-scm.com/): Used toodownload hello project from GitHub.</span></span>
* <span data-ttu-id="0a0cd-119">Egy **SSH** ügyfél: tooconnect toohello Linux-alapú HDInsight-fürtök használt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-119">An **SSH** client: Used tooconnect toohello Linux-based HDInsight clusters.</span></span> <span data-ttu-id="0a0cd-120">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="0a0cd-121">Nem kell egy meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-121">You do not need an existing HDInsight cluster.</span></span> <span data-ttu-id="0a0cd-122">jelen dokumentumban leírt lépések hello hozza létre a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-122">hello steps in this document create hello following resources:</span></span>
> 
> * <span data-ttu-id="0a0cd-123">Egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="0a0cd-123">An Azure Virtual Network</span></span>
> * <span data-ttu-id="0a0cd-124">A Storm on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)</span><span class="sxs-lookup"><span data-stu-id="0a0cd-124">A Storm on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="0a0cd-125">HBase on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)</span><span class="sxs-lookup"><span data-stu-id="0a0cd-125">An HBase on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="0a0cd-126">Az Azure Web Apps, amelyen hello webes irányítópult</span><span class="sxs-lookup"><span data-stu-id="0a0cd-126">An Azure Web App that hosts hello web dashboard</span></span>

## <a name="architecture"></a><span data-ttu-id="0a0cd-127">Architektúra</span><span class="sxs-lookup"><span data-stu-id="0a0cd-127">Architecture</span></span>

![architektúra ábrája](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

<span data-ttu-id="0a0cd-129">Ebben a példában a következő összetevők hello áll:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-129">This example consists of hello following components:</span></span>

* <span data-ttu-id="0a0cd-130">**Az Azure Event Hubs**: az érzékelők összegyűjtött adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-130">**Azure Event Hubs**: Contains data that is collected from sensors.</span></span>
* <span data-ttu-id="0a0cd-131">**A HDInsight alatt futó Storm**: az Eseményközpont kiválasztásával valós idejű adatok feldolgozása biztosít.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-131">**Storm on HDInsight**: Provides real-time processing of data from Event Hub.</span></span>
* <span data-ttu-id="0a0cd-132">**A HDInsight HBase**: biztosítja a állandó NoSQL-adattár adatok feldolgozása a Storm után.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-132">**HBase on HDInsight**: Provides a persistent NoSQL data store for data after it has been processed by Storm.</span></span>
* <span data-ttu-id="0a0cd-133">**Az Azure virtuális hálózat szolgáltatás**: lehetővé teszi, hogy a biztonságos kommunikáció közötti hello HDInsight alatt futó Storm és HBase a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-133">**Azure Virtual Network service**: Enables secure communications between hello Storm on HDInsight and HBase on HDInsight clusters.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="0a0cd-134">A virtuális hálózati hello Java HBase ügyfél API használata esetén szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-134">A virtual network is required when using hello Java HBase client API.</span></span> <span data-ttu-id="0a0cd-135">Nincs felfedve hello nyilvános átjáró a HBase fürtök keresztül.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-135">It is not exposed over hello public gateway for HBase clusters.</span></span> <span data-ttu-id="0a0cd-136">Hello lehetővé teszi a virtuális hálózaton történő telepítésekor a HBase és a Storm fürtök hello Storm fürthöz (vagy más rendszereit hello virtuális hálózaton) toodirectly ügyfél API-val HBase eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-136">Installing HBase and Storm clusters into hello same virtual network allows hello Storm cluster (or any other system on hello virtual network) toodirectly access HBase using client API.</span></span>

* <span data-ttu-id="0a0cd-137">**Irányítópult webhely**: egy példa-irányítópultot, amely valós idejű adatok diagramokat.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-137">**Dashboard website**: An example dashboard that charts data in real time.</span></span>
  
  * <span data-ttu-id="0a0cd-138">hello webhely node.js valósul meg.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-138">hello website is implemented in Node.js.</span></span>
  * <span data-ttu-id="0a0cd-139">[Socket.IO](http://socket.io/) hello Storm-topológia és hello webhely közötti valós idejű kommunikációhoz használatos.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-139">[Socket.io](http://socket.io/) is used for real-time communication between hello Storm topology and hello website.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0a0cd-140">Egy megvalósítási részletes Socket.io segítségével kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-140">Using Socket.io for communication is an implementation detail.</span></span> <span data-ttu-id="0a0cd-141">Bármely kommunikációs keretrendszert, például a nyers websocket elemek vagy SignalR is használhatja.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-141">You can use any communications framework, such as raw WebSockets or SignalR.</span></span>

  * <span data-ttu-id="0a0cd-142">[D3.js](http://d3js.org/) használt toograph hello toohello webhely küldött adatok van.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-142">[D3.js](http://d3js.org/) is used toograph hello data that is sent toohello website.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a0cd-143">Két fürt szükség, mert nem támogatott metódus toocreate egy HDInsight-fürt Storm és HBase.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-143">Two clusters are required, as there is no supported method toocreate one HDInsight cluster for both Storm and HBase.</span></span>

<span data-ttu-id="0a0cd-144">hello topológia olvassa be az adatokat az Event Hubs hello segítségével [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) osztály és írt adatok hello használata HBase [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) az osztály.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-144">hello topology reads data from Event Hub by using hello [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) class, and writes data into HBase using hello [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) class.</span></span> <span data-ttu-id="0a0cd-145">Kommunikáció a hello webhely végezhető el [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-145">Communication with hello website is accomplished by using [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span></span>

<span data-ttu-id="0a0cd-146">a következő diagram hello hello topológia elrendezését hello ismerteti:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-146">hello following diagram explains hello layout of hello topology:</span></span>

![topológia ábrája](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> <span data-ttu-id="0a0cd-148">Ez a diagram hello topológia egyszerűsített nézete.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-148">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="0a0cd-149">Az egyes összetevők példánya létrejön az Eseményközpont minden partícióján.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-149">An instance of each component is created for each partition in your Event Hub.</span></span> <span data-ttu-id="0a0cd-150">Ezek a példányok elosztott hello hello fürt csomópontja, és az adatok az alábbiak szerint történik közöttük:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-150">These instances are distributed across hello nodes in hello cluster, and data is routed between them as follows:</span></span>
> 
> * <span data-ttu-id="0a0cd-151">Hello spout toohello elemző adatait az elosztott terhelésű.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-151">Data from hello spout toohello parser is load balanced.</span></span>
> * <span data-ttu-id="0a0cd-152">Hello elemző toohello irányítópult és a HBase adatokból Eszközazonosító szerint vannak csoportosítva, így mindig a hello ugyanarra az eszközre érkező üzenetek flow toohello adott összetevő.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-152">Data from hello parser toohello Dashboard and HBase is grouped by Device ID, so that messages from hello same device always flow toohello same component.</span></span>

### <a name="topology-components"></a><span data-ttu-id="0a0cd-153">Topológia összetevők</span><span class="sxs-lookup"><span data-stu-id="0a0cd-153">Topology components</span></span>

* <span data-ttu-id="0a0cd-154">**Event Hub Spout**: hello spout rendszer alatt futó Apache Storm-verziójú 0.10.0-s megadott és magasabb.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-154">**Event Hub Spout**: hello spout is provided as part of Apache Storm version 0.10.0 and higher.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="0a0cd-155">Ebben a példában használt hello Event Hub spout egy Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6 igényel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-155">hello Event Hub spout used in this example requires a Storm on HDInsight cluster version 3.5 or 3.6.</span></span>

* <span data-ttu-id="0a0cd-156">**ParserBolt.java**: hello adatok hello spout által kibocsátott nyers JSON, és esetenként egynél több esemény is ki lesz adva egyszerre.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-156">**ParserBolt.java**: hello data that is emitted by hello spout is raw JSON, and occasionally more than one event is emitted at a time.</span></span> <span data-ttu-id="0a0cd-157">Tartalmaz a bolt hello által kibocsátott hello adatok spout és elemez JSON üdvözlőüzenetére olvassa be.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-157">This bolt reads hello data emitted by hello spout and parses hello JSON message.</span></span> <span data-ttu-id="0a0cd-158">hello bolt több mezőt tartalmazó rekordot, majd hello adatok bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-158">hello bolt then emits hello data as a tuple that contains multiple fields.</span></span>
* <span data-ttu-id="0a0cd-159">**DashboardBolt.java**: Ez az összetevő bemutatja, hogyan toouse hello Socket.io ügyféloldali kódtára a Java toosend adatok valós idejű toohello webes irányítópulton.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-159">**DashboardBolt.java**: This component demonstrates how toouse hello Socket.io client library for Java toosend data in real time toohello web dashboard.</span></span>
* <span data-ttu-id="0a0cd-160">**nem-hbase.yaml**: hello helyi módban történő futtatásakor használt topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-160">**no-hbase.yaml**: hello topology definition used when running in local mode.</span></span> <span data-ttu-id="0a0cd-161">A HBase-összetevők nem használ.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-161">It does not use HBase components.</span></span>
* <span data-ttu-id="0a0cd-162">**a hbase.yaml**: hello hello topológia hello fürtön futtatásakor használt topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-162">**with-hbase.yaml**: hello topology definition used when running hello topology on hello cluster.</span></span> <span data-ttu-id="0a0cd-163">Az a HBase összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-163">It does use HBase components.</span></span>
* <span data-ttu-id="0a0cd-164">**dev.Properties**: hello hello Event Hub spout, HBase bolt és Irányítópult-összetevők beállításait.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-164">**dev.properties**: hello configuration information for hello Event Hub spout, HBase bolt, and dashboard components.</span></span>

## <a name="prepare-your-environment"></a><span data-ttu-id="0a0cd-165">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-165">Prepare your environment</span></span>

<span data-ttu-id="0a0cd-166">Ebben a példában használata előtt létre kell hoznia egy Azure Eseményközponthoz, mely hello Storm-topológia olvassa be.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-166">Before you use this example, you must create an Azure Event Hub, which hello Storm topology reads from.</span></span>

### <a name="configure-event-hub"></a><span data-ttu-id="0a0cd-167">Az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-167">Configure Event Hub</span></span>

<span data-ttu-id="0a0cd-168">Az Event Hubs hello adatforrás ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-168">Event Hub is hello data source for this example.</span></span> <span data-ttu-id="0a0cd-169">A következő lépéseket toocreate Eseményközpontban hello használata.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-169">Use hello following steps toocreate an Event Hub.</span></span>

1. <span data-ttu-id="0a0cd-170">A hello [Azure-portálon](https://portal.azure.com), jelölje be **+ új** -> **az eszközök internetes hálózatát** -> **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-170">From hello [Azure portal](https://portal.azure.com), select **+ New** -> **Internet of Things** -> **Event Hubs**.</span></span>
2. <span data-ttu-id="0a0cd-171">A hello **létrehozása Namespace** csoportjában hajtsa végre a következő feladatok hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-171">In hello **Create Namespace** section, perform hello following tasks:</span></span>
   
   1. <span data-ttu-id="0a0cd-172">Adjon meg egy **neve** hello névtérhez.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-172">Enter a **Name** for hello namespace.</span></span>
   2. <span data-ttu-id="0a0cd-173">Válasszon tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-173">Select a pricing tier.</span></span> <span data-ttu-id="0a0cd-174">**Alapszintű** elegendő-e ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-174">**Basic** is sufficient for this example.</span></span>
   3. <span data-ttu-id="0a0cd-175">Jelölje be hello Azure **előfizetés** toouse.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-175">Select hello Azure **Subscription** toouse.</span></span>
   4. <span data-ttu-id="0a0cd-176">Válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-176">Either select an existing resource group or create a new one.</span></span>
   5. <span data-ttu-id="0a0cd-177">Jelölje be hello **hely** az Eseményközpont hello.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-177">Select hello **Location** for hello Event Hub.</span></span>
   6. <span data-ttu-id="0a0cd-178">Válassza ki **PIN-kód toodashboard**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-178">Select **Pin toodashboard**, and then click **Create**.</span></span>

3. <span data-ttu-id="0a0cd-179">Hello létrehozási folyamat befejeződik, a névtér az Event Hubs adatairól hello jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-179">When hello creation process completes, hello Event Hubs information for your namespace is displayed.</span></span> <span data-ttu-id="0a0cd-180">Itt válassza **+ Hozzáadás Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-180">From here, select **+ Add Event Hub**.</span></span> <span data-ttu-id="0a0cd-181">A hello **Eseményközpont létrehozása** területen adjon meg egy **sensordata**, majd válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-181">In hello **Create Event Hub** section, enter a name of **sensordata**, and then select **Create**.</span></span> <span data-ttu-id="0a0cd-182">Hello többi mezőt hello alapértelmezett értéken hagyja.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-182">Leave hello other fields at hello default values.</span></span>
4. <span data-ttu-id="0a0cd-183">Hello Eseményközpontokból származó tekintse meg a névtér, jelölje be **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-183">From hello Event Hubs view for your namespace, select **Event Hubs**.</span></span> <span data-ttu-id="0a0cd-184">Jelölje be hello **sensordata** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-184">Select hello **sensordata** entry.</span></span>
5. <span data-ttu-id="0a0cd-185">Hello sensordata Eseményközpont, válassza ki **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-185">From hello sensordata Event Hub, select **Shared access policies**.</span></span> <span data-ttu-id="0a0cd-186">Használjon hello **+ Hozzáadás** hivatkozás tooadd hello következő házirendek:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-186">Use hello **+ Add** link tooadd hello following policies:</span></span>

    | <span data-ttu-id="0a0cd-187">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="0a0cd-187">Policy name</span></span> | <span data-ttu-id="0a0cd-188">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="0a0cd-188">Claims</span></span> |
    | ----- | ----- |
    | <span data-ttu-id="0a0cd-189">eszközök</span><span class="sxs-lookup"><span data-stu-id="0a0cd-189">devices</span></span> | <span data-ttu-id="0a0cd-190">Küldés</span><span class="sxs-lookup"><span data-stu-id="0a0cd-190">Send</span></span> |
    | <span data-ttu-id="0a0cd-191">A Storm</span><span class="sxs-lookup"><span data-stu-id="0a0cd-191">storm</span></span> | <span data-ttu-id="0a0cd-192">Figyelés</span><span class="sxs-lookup"><span data-stu-id="0a0cd-192">Listen</span></span> |

1. <span data-ttu-id="0a0cd-193">Válassza ki mindkét házirend, és jegyezze fel a hello **elsődleges kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-193">Select both policies and make a note of hello **PRIMARY KEY** value.</span></span> <span data-ttu-id="0a0cd-194">A későbbi lépésekben mindkét házirend hello érték szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-194">You need hello value for both policies in future steps.</span></span>

## <a name="download-and-configure-hello-project"></a><span data-ttu-id="0a0cd-195">Töltse le és hello projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-195">Download and configure hello project</span></span>

<span data-ttu-id="0a0cd-196">Használja a következő toodownload hello projektet a Githubról hello.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-196">Use hello following toodownload hello project from GitHub.</span></span>

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

<span data-ttu-id="0a0cd-197">Hello parancs befejezése után a következő könyvtárstruktúrát hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-197">After hello command completes, you have hello following directory structure:</span></span>

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains hello topology
            resources/
                log4j2.xml - set logging toominimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO toohello web dashboard.
        dashboard/nodejs/ - this is hello node.js web dashboard.
        SendEvents/ - utilities toosend fake sensor data.

> [!NOTE]
> <span data-ttu-id="0a0cd-198">Ez a dokumentum nem lép az ebben a példában szereplő hello kód toofull részleteit.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-198">This document does not go in toofull details of hello code included in this example.</span></span> <span data-ttu-id="0a0cd-199">Azonban hello kódot teljesen megjegyzésként.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-199">However, hello code is fully commented.</span></span>

<span data-ttu-id="0a0cd-200">tooconfigure hello projekt tooread az Eseményközpont, nyissa meg hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` fájlt, és az Event Hubs információk toohello a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-200">tooconfigure hello project tooread from Event Hub, open hello `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` file and add your Event Hub information toohello following lines:</span></span>

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a><span data-ttu-id="0a0cd-201">Fordítsa le és helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-201">Compile and test locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a0cd-202">Storm fejlesztői környezetben működő hello topológia helyileg a szükség van.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-202">Using hello topology locally requires a working Storm development environment.</span></span> <span data-ttu-id="0a0cd-203">További információkért lásd: [egy Storm-fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-203">For more information, see [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) at Apache.org.</span></span>

> [!WARNING]
> <span data-ttu-id="0a0cd-204">A Windows környezet használatakor jelenhet meg a `java.io.IOException` amikor helyileg futó hello topológia.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-204">If you are using a Windows development environment, you may receive a `java.io.IOException` when running hello topology locally.</span></span> <span data-ttu-id="0a0cd-205">Ha igen, helyezze át a toorunning hello topológia a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-205">If so, move on toorunning hello topology on HDInsight.</span></span>

<span data-ttu-id="0a0cd-206">Előtt nem hello irányítópult tooview hello kimeneti hello topológia start és az Event Hubs adatok toostore létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-206">Before testing, you must start hello dashboard tooview hello output of hello topology and generate data toostore in Event Hub.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a0cd-207">Ez a topológia hello HBase összetevő állapota nem aktív, helyi tesztelése során.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-207">hello HBase component of this topology is not active when testing locally.</span></span> <span data-ttu-id="0a0cd-208">Java API hello hello HBase fürt nem érhető el külső hello Azure Virtual Network hello fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-208">hello Java API for hello HBase cluster cannot be accessed from outside hello Azure Virtual Network that contains hello clusters.</span></span>

### <a name="start-hello-web-application"></a><span data-ttu-id="0a0cd-209">Hello webalkalmazás elindítása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-209">Start hello web application</span></span>

1. <span data-ttu-id="0a0cd-210">Nyisson meg egy parancssort, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/dashboard`.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-210">Open a command prompt and change directories too`hdinsight-eventhub-example/dashboard`.</span></span> <span data-ttu-id="0a0cd-211">A következő parancs tooinstall hello függőségek hello webes alkalmazás által igényelt hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-211">Use hello following command tooinstall hello dependencies needed by hello web application:</span></span>
   
    ```bash
    npm install
    ```

2. <span data-ttu-id="0a0cd-212">A következő parancs toostart hello webalkalmazás hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-212">Use hello following command toostart hello web application:</span></span>
   
    ```bash
    node server.js
    ```
   
    <span data-ttu-id="0a0cd-213">Megjelenik egy üzenet hasonló toohello, a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-213">You see a message similar toohello following text:</span></span>
   
        Server listening at port 3000

3. <span data-ttu-id="0a0cd-214">Nyisson meg egy webböngészőt, és írja be `http://localhost:3000/` hello címként.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-214">Open a web browser and enter `http://localhost:3000/` as hello address.</span></span> <span data-ttu-id="0a0cd-215">A kép a következő lap hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-215">A page similar toohello following image is displayed:</span></span>
   
    ![webes irányítópult](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    <span data-ttu-id="0a0cd-217">Hagyja megnyitva a parancssort.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-217">Leave this command prompt open.</span></span> <span data-ttu-id="0a0cd-218">A vizsgálatot követően használja a Ctrl-C toostop hello webkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-218">After testing, use Ctrl-C toostop hello web server.</span></span>

### <a name="generate-data"></a><span data-ttu-id="0a0cd-219">Adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-219">Generate data</span></span>

> [!NOTE]
> <span data-ttu-id="0a0cd-220">hello ebben a szakaszban található lépéseket használata Node.js, hogy bármilyen platformon is használhatók.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-220">hello steps in this section use Node.js so that they can be used on any platform.</span></span> <span data-ttu-id="0a0cd-221">Más nyelv, tekintse meg a hello `SendEvents` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-221">For other language examples, see hello `SendEvents` directory.</span></span>

1. <span data-ttu-id="0a0cd-222">Nyisson meg egy új parancssor, rendszerhéjat vagy terminált, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/SendEvents/nodejs`.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-222">Open a new prompt, shell, or terminal, and change directories too`hdinsight-eventhub-example/SendEvents/nodejs`.</span></span> <span data-ttu-id="0a0cd-223">hello alkalmazásnak szüksége tooinstall hello függőségek hello a következő parancsot használja:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-223">tooinstall hello dependencies needed by hello application, use hello following command:</span></span>

    ```bash
    npm install
    ```

2. <span data-ttu-id="0a0cd-224">Nyissa meg hello `app.js` fájlt egy szövegszerkesztőben, és hello korábban beszerzett Eseményközpont információkat:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-224">Open hello `app.js` file in a text editor and add hello Event Hub information you obtained earlier:</span></span>
   
    ```javascript
    // ServiceBus Namespace
    var namespace = 'YourNamespace';
    // Event Hub Name
    var hubname ='sensordata';
    // Shared access Policy name and key (from Event Hub configuration)
    var my_key_name = 'devices';
    var my_key = 'YourKey';
    ```
   
   > [!NOTE]
   > <span data-ttu-id="0a0cd-225">Ebben a példában feltételezzük, hogy használt `sensordata` hello az Eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-225">This example assumes that you have used `sensordata` as hello name of your Event Hub.</span></span> <span data-ttu-id="0a0cd-226">És hogy `devices` hello neveként hello házirend, amely rendelkezik egy `Send` jogcímek.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-226">And that `devices` as hello name of hello policy that has a `Send` claim.</span></span>

3. <span data-ttu-id="0a0cd-227">A következő parancs tooinsert új bejegyzést az Event Hubs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-227">Use hello following command tooinsert new entries in Event Hub:</span></span>
   
    ```bash
    node app.js
    ```
   
    <span data-ttu-id="0a0cd-228">Kimeneti hello adatokat tartalmazó több sornyi küldött tooEvent Hub jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-228">You see several lines of output that contain hello data sent tooEvent Hub:</span></span>
   
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"0","Temperature":7}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"1","Temperature":39}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"2","Temperature":86}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"3","Temperature":29}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"4","Temperature":30}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"5","Temperature":5}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"6","Temperature":24}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"7","Temperature":40}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"8","Temperature":43}
        {"TimeStamp":"2015-02-10T14:43.05.00320Z","DeviceId":"9","Temperature":84}

### <a name="build-and-start-hello-topology"></a><span data-ttu-id="0a0cd-229">Hozza létre, és indítsa el a hello topológia</span><span class="sxs-lookup"><span data-stu-id="0a0cd-229">Build and start hello topology</span></span>

1. <span data-ttu-id="0a0cd-230">Nyisson meg egy új parancssort, és módosítsa a könyvtárat túl`hdinsight-eventhub-example/TemperatureMonitor`.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-230">Open a new command prompt and change directories too`hdinsight-eventhub-example/TemperatureMonitor`.</span></span> <span data-ttu-id="0a0cd-231">toobuild és a csomag hello topológia, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-231">toobuild and package hello topology, use hello following command:</span></span> 

    ```bash
    mvn clean package
    ```

2. <span data-ttu-id="0a0cd-232">toostart hello topológia helyi módban, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-232">toostart hello topology in local mode, use hello following command:</span></span>

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * <span data-ttu-id="0a0cd-233">`--local`hello topológia helyi módban indul.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-233">`--local` starts hello topology in local mode.</span></span>
    * <span data-ttu-id="0a0cd-234">`--filter`felhasználási hello `dev.properties` toopopulate paraméterek hello topológia meghatározása a fájl.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-234">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="0a0cd-235">`resources/no-hbase.yaml`felhasználási hello `no-hbase.yaml` topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-235">`resources/no-hbase.yaml` uses hello `no-hbase.yaml` topology definition.</span></span>
 
   <span data-ttu-id="0a0cd-236">Miután elindult, hello topológia olvassa be az Eseményközpont bejegyzéseket, és elküldi azokat a helyi gépen futó toohello irányítópult.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-236">Once started, hello topology reads entries from Event Hub, and sends them toohello dashboard running on your local machine.</span></span> <span data-ttu-id="0a0cd-237">Sorok jelennek meg hello webes irányítópult a következő kép hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-237">You should see lines appear in hello web dashboard, similar toohello following image:</span></span>
   
    ![Irányítópult adatokkal](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. <span data-ttu-id="0a0cd-239">Hello irányítópult futása közben, használja a hello `node app.js` hello előző parancsot toosend új adatok tooEvent hubok lépések.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-239">While hello dashboard is running, use hello `node app.js` command from hello previous steps toosend new data tooEvent Hubs.</span></span> <span data-ttu-id="0a0cd-240">Hello hőmérséklet értékek véletlenszerűen jönnek létre, mert a hello graph tooshow nagy változások hőmérséklet frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-240">Because hello temperature values are randomly generated, hello graph should update tooshow large changes in temperature.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0a0cd-241">Hello kell **hdinsight – az eventhub-példa/SendEvents/Nodejs** directory hello használatakor `node app.js` parancsot.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-241">You must be in hello **hdinsight-eventhub-example/SendEvents/Nodejs** directory when using hello `node app.js` command.</span></span>

3. <span data-ttu-id="0a0cd-242">Az irányítópult hello frissíti, stop hello topológiájának megtekintését a Ctrl + C ellenőrzése után.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-242">After verifying that hello dashboard updates, stop hello topology using Ctrl+C.</span></span> <span data-ttu-id="0a0cd-243">Ctrl + C toostop hello helyi webkiszolgáló is használható.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-243">You can use Ctrl+C toostop hello local web server also.</span></span>

## <a name="create-a-storm-and-hbase-cluster"></a><span data-ttu-id="0a0cd-244">A Storm és HBase-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-244">Create a Storm and HBase cluster</span></span>

<span data-ttu-id="0a0cd-245">Ez a szakasz használata a lépések hello egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md) toocreate egy Azure virtuális hálózat és a Storm és HBase fürt hello virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-245">hello steps in this section use an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) toocreate an Azure Virtual Network and a Storm and HBase cluster on hello virtual network.</span></span> <span data-ttu-id="0a0cd-246">hello sablon is létrehoz az Azure Web Apps, és bele hello irányítópult másolatának telepíti.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-246">hello template also creates an Azure Web App and deploys a copy of hello dashboard into it.</span></span>

> [!NOTE]
> <span data-ttu-id="0a0cd-247">Egy virtuális hálózati szolgál, hogy a futó Storm-fürt hello hello topológia közvetlenül kommunikálhatnak hello HBase-fürtöt hello HBase Java API használatával.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-247">A virtual network is used so that hello topology running on hello Storm cluster can directly communicate with hello HBase cluster using hello HBase Java API.</span></span>

<span data-ttu-id="0a0cd-248">a dokumentumban használt hello Resource Manager-sablon a következő nyilvános blobtárolóban található **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-248">hello Resource Manager template used in this document is located in a public blob container at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span></span>

1. <span data-ttu-id="0a0cd-249">Kattintson a következő gombra toosign tooAzure a és a nyitott hello Resource Manager sablon hello Azure-portálon hello.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-249">Click hello following button toosign in tooAzure and open hello Resource Manager template in hello Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy tooAzure"></a>

2. <span data-ttu-id="0a0cd-250">A hello **egyéni telepítési** területen adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-250">From hello **Custom deployment** section, enter hello following values:</span></span>
   
    ![A HDInsight-paraméterek](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * <span data-ttu-id="0a0cd-252">**Fürt neve kiinduló**: Ez az érték neveként hello alapszintű hello Storm és HBase fürtök használható.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-252">**Base Cluster Name**: This value is used as hello base name for hello Storm and HBase clusters.</span></span> <span data-ttu-id="0a0cd-253">Ha például **abc** hoz létre egy Storm-fürt nevű **storm-abc** és nevű HBase-fürtöt **hbase-abc**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-253">For example, entering **abc** creates a Storm cluster named **storm-abc** and an HBase cluster named **hbase-abc**.</span></span>
   * <span data-ttu-id="0a0cd-254">**A fürt bejelentkezési felhasználónevét**: hello rendszergazda felhasználóneve hello Storm és HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-254">**Cluster Login User Name**: hello admin user name for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="0a0cd-255">**A fürt bejelentkezési jelszó**: hello rendszergazdai jelszóval hello Storm és HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-255">**Cluster Login Password**: hello admin user password for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="0a0cd-256">**SSH-felhasználónév**: hello SSH felhasználói toocreate hello Storm és HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-256">**SSH User Name**: hello SSH user toocreate for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="0a0cd-257">**SSH-jelszónak**: hello SSH felhasználó hello Storm és HBase fürt hello jelszavát.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-257">**SSH Password**: hello password for hello SSH user for hello Storm and HBase clusters.</span></span>
   * <span data-ttu-id="0a0cd-258">**Hely**: hello fürtök létrehozott hello régióban.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-258">**Location**: hello region that hello clusters are created in.</span></span>
     
     <span data-ttu-id="0a0cd-259">Kattintson a **OK** toosave hello paraméterek.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-259">Click **OK** toosave hello parameters.</span></span>

3. <span data-ttu-id="0a0cd-260">Használjon hello **alapjai** szakasz toocreate egy erőforráscsoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-260">Use hello **Basics** section toocreate a resource group or select an existing one.</span></span>
4. <span data-ttu-id="0a0cd-261">A hello **erőforráscsoport helye** legördülő menüben válassza hello ugyanazon a helyen, a kiválasztott hello **hely** hello paraméterének **beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-261">In hello **Resource group location** dropdown menu, select hello same location as you selected for hello **Location** parameter in hello **Settings** section.</span></span>
5. <span data-ttu-id="0a0cd-262">Olvassa el a hello használati feltételeket, és válassza ki **toohello feltételek és kikötések fenti elfogadom**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-262">Read hello terms and conditions, and then select **I agree toohello terms and conditions stated above**.</span></span>
6. <span data-ttu-id="0a0cd-263">Végül ellenőrizze **PIN-kód toodashboard** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-263">Finally, check **Pin toodashboard** and then select **Purchase**.</span></span> <span data-ttu-id="0a0cd-264">Körülbelül 20 percet toocreate hello fürtök szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-264">It takes about 20 minutes toocreate hello clusters.</span></span>

<span data-ttu-id="0a0cd-265">Létrehozása után a hello erőforrásokat, hello erőforráscsoport információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-265">Once hello resources have been created, information about hello resource group is displayed.</span></span>

![Erőforráscsoport hello vnet és fürtök](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="0a0cd-267">Figyelje meg, hogy vannak-e a HDInsight-fürtök hello hello nevei **storm-BASENAME** és **hbase-BASENAME**, ahol BASENAME toohello sablon megadott hello nevét.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-267">Notice that hello names of hello HDInsight clusters are **storm-BASENAME** and **hbase-BASENAME**, where BASENAME is hello name you provided toohello template.</span></span> <span data-ttu-id="0a0cd-268">Ezeket a neveket az toohello fürtök kapcsolódáskor használni egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-268">You use these names in a later step when connecting toohello clusters.</span></span> <span data-ttu-id="0a0cd-269">Vegye figyelembe, hogy hello hello irányítópult hely nevét a rendszer **basename-irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-269">Also note that hello name of hello dashboard site is **basename-dashboard**.</span></span> <span data-ttu-id="0a0cd-270">Ez az érték használható a dokumentum későbbi szakaszában.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-270">This value is used later in this document.</span></span>

## <a name="configure-hello-dashboard-bolt"></a><span data-ttu-id="0a0cd-271">Hello irányítópult bolt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-271">Configure hello Dashboard bolt</span></span>

<span data-ttu-id="0a0cd-272">toosend a webes alkalmazás központilag telepített adatok toohello irányítópult, módosítania kell a következő sort a hello hello `dev.properties`fájlt:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-272">toosend data toohello dashboard deployed as a web app, you must modify hello following line in hello `dev.properties`file:</span></span>

```yaml
dashboard.uri: http://localhost:3000
```

<span data-ttu-id="0a0cd-273">Változás `http://localhost:3000` túl`http://BASENAME-dashboard.azurewebsites.net` és hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-273">Change `http://localhost:3000` too`http://BASENAME-dashboard.azurewebsites.net` and save hello file.</span></span> <span data-ttu-id="0a0cd-274">Cserélje le **BASENAME** hello előző lépésben megadott hello Alap nevű.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-274">Replace **BASENAME** with hello base name you provided in hello previous step.</span></span> <span data-ttu-id="0a0cd-275">Hello tooselect hello irányítópult és nézet hello URL-címet korábban létrehozott erőforráscsoportot is használható.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-275">You can also use hello resource group created previously tooselect hello dashboard and view hello URL.</span></span>

## <a name="create-hello-hbase-table"></a><span data-ttu-id="0a0cd-276">Hello HBase tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-276">Create hello HBase table</span></span>

<span data-ttu-id="0a0cd-277">toostore adatok a HBase, azt először létre kell hoznia egy tábla.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-277">toostore data in HBase, we must first create a table.</span></span> <span data-ttu-id="0a0cd-278">Előzetes létrehozása, amelyet a Storm a toowrite az erőforrásokat, közben toocreate erőforráshoz, a Storm-topológia több példányt próbált eredményezhet, toocreate hello ugyanazt az erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-278">Pre-create resources that Storm needs toowrite to, as trying toocreate resources from inside a Storm topology can result in multiple instances trying toocreate hello same resource.</span></span> <span data-ttu-id="0a0cd-279">Hozzon létre hello erőforrások hello topológia kívül, és a Storm olvasási/írási és elemzés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-279">Create hello resources outside hello topology and use Storm for reading/writing and analytics.</span></span>

1. <span data-ttu-id="0a0cd-280">SSH tooconnect toohello HBase-fürtöt használ hello SSH-felhasználó és a fürt létrehozása során a toohello sablon megadott jelszó segítségével.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-280">Use SSH tooconnect toohello HBase cluster using hello SSH user and password you supplied toohello template during cluster creation.</span></span> <span data-ttu-id="0a0cd-281">Például, ha a kapcsolódás hello használatával `ssh` parancs szintaxisa a következő hello szeretné használni:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-281">For example, if connecting using hello `ssh` command, you would use hello following syntax:</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    <span data-ttu-id="0a0cd-282">Cserélje le `sshuser` az SSH-felhasználónév hello hello fürt létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-282">Replace `sshuser` with hello SSH user name you provided when creating hello cluster.</span></span> <span data-ttu-id="0a0cd-283">Cserélje le `clustername` hello HBase fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-283">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="0a0cd-284">Hello SSH-munkamenetet a start hello HBase rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-284">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="0a0cd-285">Miután hello rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-285">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="0a0cd-286">Hello HBase rendszerhéj adja meg a következő parancs toocreate egy tábla toostore hello érzékelőadatait hello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-286">From hello HBase shell, enter hello following command toocreate a table toostore hello sensor data:</span></span>
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. <span data-ttu-id="0a0cd-287">Győződjön meg arról, hogy hello tábla hello a következő parancs használatával jött létre:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-287">Verify that hello table has been created by using hello following command:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="0a0cd-288">Ez visszaadja a hasonló toohello információkat a következő példa, amely azt jelzi, hogy nincsenek-e 0 sor hello tábla.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-288">This returns information similar toohello following example, indicating that there are 0 rows in hello table.</span></span>
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. <span data-ttu-id="0a0cd-289">Adja meg `exit` tooexit hello HBase rendszerhéj:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-289">Enter `exit` tooexit hello HBase shell:</span></span>

## <a name="configure-hello-hbase-bolt"></a><span data-ttu-id="0a0cd-290">Hello HBase bolt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a0cd-290">Configure hello HBase bolt</span></span>

<span data-ttu-id="0a0cd-291">a Storm-fürt hello tooHBase toowrite, meg kell adnia hello HBase bolt hello konfigurációs részleteit a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-291">toowrite tooHBase from hello Storm cluster, you must provide hello HBase bolt with hello configuration details of your HBase cluster.</span></span>

1. <span data-ttu-id="0a0cd-292">A következő példák tooretrieve hello Zookeeper a HBase fürt kvórumát hello egyikét használja:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-292">Use one of hello following examples tooretrieve hello Zookeeper quorum for your HBase cluster:</span></span>

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > <span data-ttu-id="0a0cd-293">Cserélje le `your_HDInsight_cluster_name` hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-293">Replace `your_HDInsight_cluster_name` with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="0a0cd-294">Hello telepítéséről további információt `jq` segédprogram, lásd: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-294">For more information on installing hello `jq` utility, see [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span></span>
    >
    > <span data-ttu-id="0a0cd-295">Amikor a rendszer kéri, adja meg hello jelszó hello HDInsight rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-295">When prompted, enter hello password for hello HDInsight admin login.</span></span>

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > <span data-ttu-id="0a0cd-296">Cserélje le a(z) your_HDInsight_cluster_name hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-296">Replace \`your_HDInsight_cluster_name with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="0a0cd-297">Amikor a rendszer kéri, adja meg hello jelszó hello HDInsight rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-297">When prompted, enter hello password for hello HDInsight admin login.</span></span>
    >
    > <span data-ttu-id="0a0cd-298">Ebben a példában az Azure PowerShell szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-298">This example requires Azure PowerShell.</span></span> <span data-ttu-id="0a0cd-299">További információ az Azure PowerShell használatával, lásd: [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span><span class="sxs-lookup"><span data-stu-id="0a0cd-299">For more information on using Azure PowerShell, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span></span>

    <span data-ttu-id="0a0cd-300">Ezekben a példákban által visszaadott hello információkat a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-300">hello information returned by these examples is similar toohello following text:</span></span>

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    <span data-ttu-id="0a0cd-301">Ez az információ Storm toocommunicate hello HBase fürt használja.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-301">This information is used by Storm toocommunicate with hello HBase cluster.</span></span>

2. <span data-ttu-id="0a0cd-302">Módosítsa a hello `dev.properties` fájlt, és hello Zookeeper kvórum információk toohello a következő sort:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-302">Modify hello `dev.properties` file and add hello Zookeeper quorum information toohello following line:</span></span>

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-hello-solution-toohdinsight"></a><span data-ttu-id="0a0cd-303">Hozza létre, csomagot, és hello megoldás tooHDInsight telepítése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-303">Build, package, and deploy hello solution tooHDInsight</span></span>

<span data-ttu-id="0a0cd-304">A fejlesztői környezetben használja a következő lépéseket toodeploy hello Storm topology toohello storm-fürt hello.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-304">In your development environment, use hello following steps toodeploy hello Storm topology toohello storm cluster.</span></span>

1. <span data-ttu-id="0a0cd-305">A hello `TemperatureMonitor` könyvtárába, használjon hello következő tooperform új buildverziót parancsot, és a projektből JAR-csomag létrehozása:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-305">From hello `TemperatureMonitor` directory, use hello following command tooperform a new build and create a JAR package from your project:</span></span>
   
        mvn clean package
   
    <span data-ttu-id="0a0cd-306">Ez a parancs létrehoz egy nevű fájlt `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `cél "mappában található a projekthez.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-306">This command creates a file named `TemperatureMonitor-1.0-SNAPSHOT.jar in hello `target\` directory of your project.</span></span>

2. <span data-ttu-id="0a0cd-307">Használjon scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` és `dev.properties` fájlok tooyour Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-307">Use scp tooupload hello `TemperatureMonitor-1.0-SNAPSHOT.jar` and `dev.properties` files tooyour Storm cluster.</span></span> <span data-ttu-id="0a0cd-308">Hello a következő példában cserélje le `sshuser` hello fürt létrehozásakor megadott hello SSH felhasználóval és `clustername` hello nevet, a Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-308">In hello following example, replace `sshuser` with hello SSH user you provided when creating hello cluster, and `clustername` with hello name of your Storm cluster.</span></span> <span data-ttu-id="0a0cd-309">Amikor a rendszer kéri, adja meg hello jelszó hello SSH felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-309">When prompted, enter hello password for hello SSH user.</span></span>
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > <span data-ttu-id="0a0cd-310">Tooupload hello fájlok több percig is tarthat.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-310">It may take several minutes tooupload hello files.</span></span>

    <span data-ttu-id="0a0cd-311">További információk hello az `scp` és `ssh` parancsok a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](./hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="0a0cd-311">For more information on using hello `scp` and `ssh` commands with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

3. <span data-ttu-id="0a0cd-312">Hello fájl a feltöltést követően csatlakozzon az SSH használatával toohello Storm-fürt.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-312">Once hello file has been uploaded, connect toohello Storm cluster using SSH.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="0a0cd-313">Cserélje le `sshuser` hello SSH felhasználónévvel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-313">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="0a0cd-314">Cserélje le `clustername` hello Storm-fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-314">Replace `clustername` with hello Storm cluster name.</span></span>

4. <span data-ttu-id="0a0cd-315">toostart hello topológia, a következő parancsot a hello SSH-munkamenetet hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-315">toostart hello topology, use hello following command from hello SSH session:</span></span>
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * <span data-ttu-id="0a0cd-316">`--remote`elküldi a hello topológia toohello Nimbus szolgáltatást, amely elküldi toohello felügyelő hello fürt csomópontja.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-316">`--remote` submits hello topology toohello Nimbus service, which distributes it toohello supervisor nodes in hello cluster.</span></span>
    * <span data-ttu-id="0a0cd-317">`--filter`felhasználási hello `dev.properties` toopopulate paraméterek hello topológia meghatározása a fájl.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-317">`--filter` uses hello `dev.properties` file toopopulate parameters in hello topology definition.</span></span>
    * <span data-ttu-id="0a0cd-318">`-R /with-hbase.yaml`felhasználási hello `with-hbase.yaml` topológia hello csomagban található.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-318">`-R /with-hbase.yaml` uses hello `with-hbase.yaml` topology included in hello package.</span></span>

5. <span data-ttu-id="0a0cd-319">Miután hello topológia elindult, nyisson meg egy böngésző toohello webhelyen közzétett Azure, majd használja hello `node app.js` parancs toosend adatok tooEvent központ.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-319">After hello topology has started, open a browser toohello website you published on Azure, then use hello `node app.js` command toosend data tooEvent Hub.</span></span> <span data-ttu-id="0a0cd-320">Hello webes irányítópult frissítés toodisplay hello adatokat kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-320">You should see hello web dashboard update toodisplay hello information.</span></span>
   
    ![irányítópult](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a><span data-ttu-id="0a0cd-322">A HBase-adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-322">View HBase data</span></span>

<span data-ttu-id="0a0cd-323">A következő lépéseket tooconnect tooHBase hello használja, és győződjön meg arról, hogy hello adatok írt toohello táblázat:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-323">Use hello following steps tooconnect tooHBase and verify that hello data has been written toohello table:</span></span>

1. <span data-ttu-id="0a0cd-324">SSH tooconnect toohello HBase-fürtöt használ.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-324">Use SSH tooconnect toohello HBase cluster.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="0a0cd-325">Cserélje le `sshuser` hello SSH felhasználónévvel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-325">Replace `sshuser` with hello SSH user name.</span></span> <span data-ttu-id="0a0cd-326">Cserélje le `clustername` hello HBase fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-326">Replace `clustername` with hello HBase cluster name.</span></span>

2. <span data-ttu-id="0a0cd-327">Hello SSH-munkamenetet a start hello HBase rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-327">From hello SSH session, start hello HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="0a0cd-328">Miután hello rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-328">Once hello shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="0a0cd-329">Hello tábla azon sorait megtekintése:</span><span class="sxs-lookup"><span data-stu-id="0a0cd-329">View rows from hello table:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="0a0cd-330">Ez a parancs visszaadja a hasonló toohello információkat a következő szöveg, jelezve, hogy van-e adatok hello tábla.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-330">This command returns information similar toohello following text, indicating that there is data in hello table.</span></span>
   
        hbase(main):002:0> scan 'SensorData'
        ROW                             COLUMN+CELL
        \x00\x00\x00\x00               column=cf:temperature, timestamp=1467290788277, value=\x00\x00\x00\x04
        \x00\x00\x00\x00               column=cf:timestamp, timestamp=1467290788277, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x01               column=cf:temperature, timestamp=1467290788348, value=\x00\x00\x00M
        \x00\x00\x00\x01               column=cf:timestamp, timestamp=1467290788348, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x02               column=cf:temperature, timestamp=1467290788268, value=\x00\x00\x00R
        \x00\x00\x00\x02               column=cf:timestamp, timestamp=1467290788268, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x03               column=cf:temperature, timestamp=1467290788269, value=\x00\x00\x00#
        \x00\x00\x00\x03               column=cf:timestamp, timestamp=1467290788269, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x04               column=cf:temperature, timestamp=1467290788356, value=\x00\x00\x00>
        \x00\x00\x00\x04               column=cf:timestamp, timestamp=1467290788356, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x05               column=cf:temperature, timestamp=1467290788326, value=\x00\x00\x00\x0D
        \x00\x00\x00\x05               column=cf:timestamp, timestamp=1467290788326, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x06               column=cf:temperature, timestamp=1467290788253, value=\x00\x00\x009
        \x00\x00\x00\x06               column=cf:timestamp, timestamp=1467290788253, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x07               column=cf:temperature, timestamp=1467290788229, value=\x00\x00\x00\x12
        \x00\x00\x00\x07               column=cf:timestamp, timestamp=1467290788229, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x08               column=cf:temperature, timestamp=1467290788336, value=\x00\x00\x00\x16
        \x00\x00\x00\x08               column=cf:timestamp, timestamp=1467290788336, value=2015-02-10T14:43.05.00320Z
        \x00\x00\x00\x09               column=cf:temperature, timestamp=1467290788246, value=\x00\x00\x001
        \x00\x00\x00\x09               column=cf:timestamp, timestamp=1467290788246, value=2015-02-10T14:43.05.00320Z
        10 row(s) in 0.1800 seconds
   
   > [!NOTE]
   > <span data-ttu-id="0a0cd-331">A vizsgálati művelet legfeljebb 10 sor hello táblát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-331">This scan operation returns a maximum of 10 rows from hello table.</span></span>

## <a name="delete-your-clusters"></a><span data-ttu-id="0a0cd-332">A fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="0a0cd-332">Delete your clusters</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="0a0cd-333">toodelete hello fürt, tárolás és a webes alkalmazás egyszerre, törölje a hello csoportot, amely tartalmazza azokat.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-333">toodelete hello clusters, storage, and web app at one time, delete hello resource group that contains them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a0cd-334">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a0cd-334">Next steps</span></span>

<span data-ttu-id="0a0cd-335">A HDInsight Storm-topológiák további példákért lásd [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md)</span><span class="sxs-lookup"><span data-stu-id="0a0cd-335">For more examples of Storm topologies with HDInsight, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md)</span></span>

<span data-ttu-id="0a0cd-336">Apache Storm kapcsolatos további információkért lásd: hello [alatt futó Apache Storm](https://storm.incubator.apache.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-336">For more information about Apache Storm, see hello [Apache Storm](https://storm.incubator.apache.org/) site.</span></span>

<span data-ttu-id="0a0cd-337">A HDInsight HBase kapcsolatos további információkért lásd: hello [HBase a HDInsight áttekintése](hdinsight-hbase-overview.md).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-337">For more information about HBase on HDInsight, see hello [HBase with HDInsight Overview](hdinsight-hbase-overview.md).</span></span>

<span data-ttu-id="0a0cd-338">Socket.io kapcsolatos további információkért lásd: hello [socket.io](http://socket.io/) hely.</span><span class="sxs-lookup"><span data-stu-id="0a0cd-338">For more information about Socket.io, see hello [socket.io](http://socket.io/) site.</span></span>

<span data-ttu-id="0a0cd-339">D3.js kapcsolatos további információkért lásd: [D3.js - adatokat tartalmazó dokumentumok vezérelt](http://d3js.org/).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-339">For more information about D3.js, see [D3.js - Data Driven Documents](http://d3js.org/).</span></span>

<span data-ttu-id="0a0cd-340">A Java topológiák létrehozásával kapcsolatos további információkért lásd: [HDInsight alatt futó Apache Storm topológiák fejlesztése Java](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-340">For information about creating topologies in Java, see [Develop Java topologies for Apache Storm on HDInsight](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="0a0cd-341">A .NET topológiák létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák futó Apache stormra a Visual Studio használatával HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="0a0cd-341">For information about creating topologies in .NET, see [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

[azure-portal]: https://portal.azure.com
