---
title: "Az Apache Storm és HBase érzékelőadatok elemzése |} Microsoft Docs"
description: "Ismerje meg, hogyan csatlakozhat az Apache Storm egy virtuális hálózattal. Használjon Storm HBase használatával dolgozza az eseményközpontban lévő és jelenítheti meg azt a D3.js."
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
ms.openlocfilehash: 0d1cc959c87bd64ed728f8b56c9b9156fa492a8b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="analyze-sensor-data-with-apache-storm-event-hub-and-hbase-in-hdinsight-hadoop"></a><span data-ttu-id="fdcb3-104">Apache Storm, az Event Hubs és a HBase a HDInsight (Hadoop) érzékelőadatok elemzése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-104">Analyze sensor data with Apache Storm, Event Hub, and HBase in HDInsight (Hadoop)</span></span>

<span data-ttu-id="fdcb3-105">Útmutató Apache Storm on HDInsight használatával dolgozza az Azure Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-105">Learn how to use Apache Storm on HDInsight to process sensor data from Azure Event Hub.</span></span> <span data-ttu-id="fdcb3-106">Az adatok az Apache HBase a HDInsight majd tárolja, és ábrázolt D3.js használatával.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-106">The data is then stored into Apache HBase on HDInsight, and visualized using D3.js.</span></span>

<span data-ttu-id="fdcb3-107">Ebben a dokumentumban használt Azure Resource Manager sablon bemutatja, hogyan hozzon létre több Azure-erőforrások erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-107">The Azure Resource Manager template used in this document demonstrates how to create multiple Azure resources in a resource group.</span></span> <span data-ttu-id="fdcb3-108">A sablon egy Azure virtuális hálózatra, két HDInsight-fürtök (a Storm és HBase) és az Azure Web Apps hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-108">The template creates an Azure Virtual Network, two HDInsight clusters (Storm and HBase) and an Azure Web App.</span></span> <span data-ttu-id="fdcb3-109">A valós idejű webes irányítópult node.js végrehajtásának automatikusan telepítve van a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-109">A node.js implementation of a real-time web dashboard is automatically deployed to the web app.</span></span>

> [!NOTE]
> <span data-ttu-id="fdcb3-110">A dokumentum és példa ebben a dokumentumban szereplő információk a HDInsight 3.6 verzió szükséges.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-110">The information in this document and example in this document require HDInsight version 3.6.</span></span>
>
> <span data-ttu-id="fdcb3-111">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fdcb3-112">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdcb3-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fdcb3-113">Prerequisites</span></span>

* <span data-ttu-id="fdcb3-114">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-114">An Azure subscription.</span></span>
* <span data-ttu-id="fdcb3-115">[NODE.js](http://nodejs.org/): a webes irányítópult helyileg a fejlesztési környezetet az előzetes használt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-115">[Node.js](http://nodejs.org/): Used to preview the web dashboard locally on your development environment.</span></span>
* <span data-ttu-id="fdcb3-116">[Java és a JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): a Storm-topológiák fejlesztése használatával.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-116">[Java and the JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/index.html): Used to develop the Storm topology.</span></span>
* <span data-ttu-id="fdcb3-117">[Maven](http://maven.apache.org/what-is-maven.html): build és a projekt lefordítása használt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-117">[Maven](http://maven.apache.org/what-is-maven.html): Used to build and compile the project.</span></span>
* <span data-ttu-id="fdcb3-118">[Git](http://git-scm.com/): tölthető le a projektet a Githubról.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-118">[Git](http://git-scm.com/): Used to download the project from GitHub.</span></span>
* <span data-ttu-id="fdcb3-119">Egy **SSH** ügyfél: a Linux-alapú HDInsight-fürtök való kapcsolódáshoz használt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-119">An **SSH** client: Used to connect to the Linux-based HDInsight clusters.</span></span> <span data-ttu-id="fdcb3-120">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-120">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="fdcb3-121">Nem kell egy meglévő HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-121">You do not need an existing HDInsight cluster.</span></span> <span data-ttu-id="fdcb3-122">A jelen dokumentumban leírt lépések hozza létre a következőket:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-122">The steps in this document create the following resources:</span></span>
> 
> * <span data-ttu-id="fdcb3-123">Egy Azure virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="fdcb3-123">An Azure Virtual Network</span></span>
> * <span data-ttu-id="fdcb3-124">A Storm on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)</span><span class="sxs-lookup"><span data-stu-id="fdcb3-124">A Storm on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="fdcb3-125">HBase on HDInsight-fürt (Linux-alapú, két munkavégző csomópontokhoz)</span><span class="sxs-lookup"><span data-stu-id="fdcb3-125">An HBase on HDInsight cluster (Linux-based, two worker nodes)</span></span>
> * <span data-ttu-id="fdcb3-126">Az Azure Web Apps, amelyen a webhely irányítópult</span><span class="sxs-lookup"><span data-stu-id="fdcb3-126">An Azure Web App that hosts the web dashboard</span></span>

## <a name="architecture"></a><span data-ttu-id="fdcb3-127">Architektúra</span><span class="sxs-lookup"><span data-stu-id="fdcb3-127">Architecture</span></span>

![architektúra ábrája](./media/hdinsight-storm-sensor-data-analysis/devicesarchitecture.png)

<span data-ttu-id="fdcb3-129">Ebben a példában a következő összetevőkből áll:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-129">This example consists of the following components:</span></span>

* <span data-ttu-id="fdcb3-130">**Az Azure Event Hubs**: az érzékelők összegyűjtött adatokat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-130">**Azure Event Hubs**: Contains data that is collected from sensors.</span></span>
* <span data-ttu-id="fdcb3-131">**A HDInsight alatt futó Storm**: az Eseményközpont kiválasztásával valós idejű adatok feldolgozása biztosít.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-131">**Storm on HDInsight**: Provides real-time processing of data from Event Hub.</span></span>
* <span data-ttu-id="fdcb3-132">**A HDInsight HBase**: biztosítja a állandó NoSQL-adattár adatok feldolgozása a Storm után.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-132">**HBase on HDInsight**: Provides a persistent NoSQL data store for data after it has been processed by Storm.</span></span>
* <span data-ttu-id="fdcb3-133">**Az Azure virtuális hálózat szolgáltatás**: lehetővé teszi, hogy a HDInsight alatt futó Storm és HBase a HDInsight-fürtök közötti biztonságos kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-133">**Azure Virtual Network service**: Enables secure communications between the Storm on HDInsight and HBase on HDInsight clusters.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="fdcb3-134">A virtuális hálózatra azért szükség, a Java HBase ügyfél API használata esetén.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-134">A virtual network is required when using the Java HBase client API.</span></span> <span data-ttu-id="fdcb3-135">Nincs felfedve, a HBase fürtök nyilvános átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-135">It is not exposed over the public gateway for HBase clusters.</span></span> <span data-ttu-id="fdcb3-136">A HBase és a Storm-fürtök telepítését az azonos virtuális hálózatban lehetővé teszi a Storm-fürt (vagy a virtuális hálózaton lévő más rendszereit) közvetlen hozzáférést a HBase ügyfél API-val.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-136">Installing HBase and Storm clusters into the same virtual network allows the Storm cluster (or any other system on the virtual network) to directly access HBase using client API.</span></span>

* <span data-ttu-id="fdcb3-137">**Irányítópult webhely**: egy példa-irányítópultot, amely valós idejű adatok diagramokat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-137">**Dashboard website**: An example dashboard that charts data in real time.</span></span>
  
  * <span data-ttu-id="fdcb3-138">A webhely a node.js valósul meg.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-138">The website is implemented in Node.js.</span></span>
  * <span data-ttu-id="fdcb3-139">[Socket.IO](http://socket.io/) valós idejű, a Storm-topológia és a webhely közötti kommunikációhoz használt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-139">[Socket.io](http://socket.io/) is used for real-time communication between the Storm topology and the website.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="fdcb3-140">Egy megvalósítási részletes Socket.io segítségével kommunikációra.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-140">Using Socket.io for communication is an implementation detail.</span></span> <span data-ttu-id="fdcb3-141">Bármely kommunikációs keretrendszert, például a nyers websocket elemek vagy SignalR is használhatja.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-141">You can use any communications framework, such as raw WebSockets or SignalR.</span></span>

  * <span data-ttu-id="fdcb3-142">[D3.js](http://d3js.org/) szolgál a webhely elküldött adatblokkhoz diagramot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-142">[D3.js](http://d3js.org/) is used to graph the data that is sent to the website.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdcb3-143">Két fürt nem szükséges, mivel nincs támogatott módszer Storm és HBase egy HDInsight-fürt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-143">Two clusters are required, as there is no supported method to create one HDInsight cluster for both Storm and HBase.</span></span>

<span data-ttu-id="fdcb3-144">A topológia olvassa be az adatokat az Event Hubs használatával a [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) osztály és HBase használatával írt adatok a [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) osztály.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-144">The topology reads data from Event Hub by using the [org.apache.storm.eventhubs.spout.EventHubSpout](http://storm.apache.org/releases/0.10.1/javadocs/org/apache/storm/eventhubs/spout/class-use/EventHubSpout.html) class, and writes data into HBase using the [org.apache.storm.hbase.bolt.HBaseBolt](https://storm.apache.org/releases/1.0.1/javadocs/org/apache/storm/hbase/bolt/HBaseBolt.html) class.</span></span> <span data-ttu-id="fdcb3-145">A webhely kommunikál végezhető el [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-145">Communication with the website is accomplished by using [socket.io-client.java](https://github.com/nkzawa/socket.io-client.java).</span></span>

<span data-ttu-id="fdcb3-146">A következő ábra illusztrálja a topológia elrendezését:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-146">The following diagram explains the layout of the topology:</span></span>

![topológia ábrája](./media/hdinsight-storm-sensor-data-analysis/sensoranalysis.png)

> [!NOTE]
> <span data-ttu-id="fdcb3-148">Ez az ábra a topológia egyszerűsített nézete.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-148">This diagram is a simplified view of the topology.</span></span> <span data-ttu-id="fdcb3-149">Az egyes összetevők példánya létrejön az Eseményközpont minden partícióján.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-149">An instance of each component is created for each partition in your Event Hub.</span></span> <span data-ttu-id="fdcb3-150">Ezek a példányok elosztott a fürt csomópontjaihoz, és adatok között irányított azokat az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-150">These instances are distributed across the nodes in the cluster, and data is routed between them as follows:</span></span>
> 
> * <span data-ttu-id="fdcb3-151">Az elemző a spout adatainak terhelésű.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-151">Data from the spout to the parser is load balanced.</span></span>
> * <span data-ttu-id="fdcb3-152">Az irányítópult és a HBase az elemző adatainak Eszközazonosító, szerint van csoportosítva, így ugyanarra az eszközre érkező üzenetek mindig azonos összetevőnek.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-152">Data from the parser to the Dashboard and HBase is grouped by Device ID, so that messages from the same device always flow to the same component.</span></span>

### <a name="topology-components"></a><span data-ttu-id="fdcb3-153">Topológia összetevők</span><span class="sxs-lookup"><span data-stu-id="fdcb3-153">Topology components</span></span>

* <span data-ttu-id="fdcb3-154">**Event Hub Spout**: A spout rendszer alatt futó Apache Storm-verziójú 0.10.0-s megadott és magasabb.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-154">**Event Hub Spout**: The spout is provided as part of Apache Storm version 0.10.0 and higher.</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="fdcb3-155">Az ebben a példában használt Event Hub spout egy Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6 igényel.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-155">The Event Hub spout used in this example requires a Storm on HDInsight cluster version 3.5 or 3.6.</span></span>

* <span data-ttu-id="fdcb3-156">**ParserBolt.java**: az adatok a spout által kibocsátott nyers JSON, és esetenként egynél több esemény is ki lesz adva egyszerre.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-156">**ParserBolt.java**: The data that is emitted by the spout is raw JSON, and occasionally more than one event is emitted at a time.</span></span> <span data-ttu-id="fdcb3-157">Tartalmaz a bolt beolvassa a spout által kibocsátott adatokról, és elemzi a JSON-üzenet.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-157">This bolt reads the data emitted by the spout and parses the JSON message.</span></span> <span data-ttu-id="fdcb3-158">A bolt több mezőt tartalmazó rekordot, majd az adatok bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-158">The bolt then emits the data as a tuple that contains multiple fields.</span></span>
* <span data-ttu-id="fdcb3-159">**DashboardBolt.java**: Ez az összetevő bemutatja, hogyan kell a Socket.io ügyféloldali kódtára a Java használatával valós idejű adatokat küldeni a webes irányítópult.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-159">**DashboardBolt.java**: This component demonstrates how to use the Socket.io client library for Java to send data in real time to the web dashboard.</span></span>
* <span data-ttu-id="fdcb3-160">**nem-hbase.yaml**: helyi módban történő futtatásakor használt topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-160">**no-hbase.yaml**: The topology definition used when running in local mode.</span></span> <span data-ttu-id="fdcb3-161">A HBase-összetevők nem használ.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-161">It does not use HBase components.</span></span>
* <span data-ttu-id="fdcb3-162">**a hbase.yaml**: használható, ha a topológia a fürtben futó topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-162">**with-hbase.yaml**: The topology definition used when running the topology on the cluster.</span></span> <span data-ttu-id="fdcb3-163">Az a HBase összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-163">It does use HBase components.</span></span>
* <span data-ttu-id="fdcb3-164">**dev.Properties**: A konfigurációs adatokat az Event Hub spout, a HBase bolt és az Irányítópult-összetevők.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-164">**dev.properties**: The configuration information for the Event Hub spout, HBase bolt, and dashboard components.</span></span>

## <a name="prepare-your-environment"></a><span data-ttu-id="fdcb3-165">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-165">Prepare your environment</span></span>

<span data-ttu-id="fdcb3-166">Ez a példa használata előtt létre kell hoznia egy Azure Event hubs Eseményközpontot, amelyben a Storm-topológia a.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-166">Before you use this example, you must create an Azure Event Hub, which the Storm topology reads from.</span></span>

### <a name="configure-event-hub"></a><span data-ttu-id="fdcb3-167">Az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-167">Configure Event Hub</span></span>

<span data-ttu-id="fdcb3-168">Az Event Hubs ebben a példában az adatforrást.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-168">Event Hub is the data source for this example.</span></span> <span data-ttu-id="fdcb3-169">Az alábbi lépések segítségével létrehoz egy Eseményközpontot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-169">Use the following steps to create an Event Hub.</span></span>

1. <span data-ttu-id="fdcb3-170">Az a [Azure-portálon](https://portal.azure.com), jelölje be **+ új** -> **az eszközök internetes hálózatát** -> **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-170">From the [Azure portal](https://portal.azure.com), select **+ New** -> **Internet of Things** -> **Event Hubs**.</span></span>
2. <span data-ttu-id="fdcb3-171">Az a **létrehozása Namespace** területen a következő feladatokat:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-171">In the **Create Namespace** section, perform the following tasks:</span></span>
   
   1. <span data-ttu-id="fdcb3-172">Adjon meg egy **neve** a névtérhez.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-172">Enter a **Name** for the namespace.</span></span>
   2. <span data-ttu-id="fdcb3-173">Válasszon tarifacsomagot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-173">Select a pricing tier.</span></span> <span data-ttu-id="fdcb3-174">**Alapszintű** elegendő-e ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-174">**Basic** is sufficient for this example.</span></span>
   3. <span data-ttu-id="fdcb3-175">Válassza ki az Azure **előfizetés** használatára.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-175">Select the Azure **Subscription** to use.</span></span>
   4. <span data-ttu-id="fdcb3-176">Válasszon ki egy meglévő erőforráscsoportot, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-176">Either select an existing resource group or create a new one.</span></span>
   5. <span data-ttu-id="fdcb3-177">Válassza ki a **hely** az Event Hubs számára.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-177">Select the **Location** for the Event Hub.</span></span>
   6. <span data-ttu-id="fdcb3-178">Válassza ki **rögzítés az irányítópulton**, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-178">Select **Pin to dashboard**, and then click **Create**.</span></span>

3. <span data-ttu-id="fdcb3-179">A létrehozási folyamat befejezését követően a névtérhez az Event Hubs információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-179">When the creation process completes, the Event Hubs information for your namespace is displayed.</span></span> <span data-ttu-id="fdcb3-180">Itt válassza **+ Hozzáadás Eseményközpont**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-180">From here, select **+ Add Event Hub**.</span></span> <span data-ttu-id="fdcb3-181">Az a **Eseményközpont létrehozása** területen adjon meg egy **sensordata**, majd válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-181">In the **Create Event Hub** section, enter a name of **sensordata**, and then select **Create**.</span></span> <span data-ttu-id="fdcb3-182">Az alapértelmezett értékeket, a többi mezőt hagyja.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-182">Leave the other fields at the default values.</span></span>
4. <span data-ttu-id="fdcb3-183">Az Event Hubs nézetből a névtérhez, válassza ki a **Event Hubs**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-183">From the Event Hubs view for your namespace, select **Event Hubs**.</span></span> <span data-ttu-id="fdcb3-184">Válassza ki a **sensordata** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-184">Select the **sensordata** entry.</span></span>
5. <span data-ttu-id="fdcb3-185">Az Event Hubs sensordata jelölje ki **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-185">From the sensordata Event Hub, select **Shared access policies**.</span></span> <span data-ttu-id="fdcb3-186">Használja a **+ Hozzáadás** szabályzatok beállítása a következő hivatkozásra:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-186">Use the **+ Add** link to add the following policies:</span></span>

    | <span data-ttu-id="fdcb3-187">Házirend neve</span><span class="sxs-lookup"><span data-stu-id="fdcb3-187">Policy name</span></span> | <span data-ttu-id="fdcb3-188">Jogcímek</span><span class="sxs-lookup"><span data-stu-id="fdcb3-188">Claims</span></span> |
    | ----- | ----- |
    | <span data-ttu-id="fdcb3-189">eszközök</span><span class="sxs-lookup"><span data-stu-id="fdcb3-189">devices</span></span> | <span data-ttu-id="fdcb3-190">Küldés</span><span class="sxs-lookup"><span data-stu-id="fdcb3-190">Send</span></span> |
    | <span data-ttu-id="fdcb3-191">A Storm</span><span class="sxs-lookup"><span data-stu-id="fdcb3-191">storm</span></span> | <span data-ttu-id="fdcb3-192">Figyelés</span><span class="sxs-lookup"><span data-stu-id="fdcb3-192">Listen</span></span> |

1. <span data-ttu-id="fdcb3-193">Válassza ki mindkét házirend, és jegyezze fel a **elsődleges kulcs** érték.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-193">Select both policies and make a note of the **PRIMARY KEY** value.</span></span> <span data-ttu-id="fdcb3-194">Az érték a későbbi lépésekben mindkét házirend van szükség.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-194">You need the value for both policies in future steps.</span></span>

## <a name="download-and-configure-the-project"></a><span data-ttu-id="fdcb3-195">Töltse le és a projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-195">Download and configure the project</span></span>

<span data-ttu-id="fdcb3-196">A következő segítségével töltse le a projektet a Githubról.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-196">Use the following to download the project from GitHub.</span></span>

    git clone https://github.com/Blackmist/hdinsight-eventhub-example

<span data-ttu-id="fdcb3-197">A parancs befejezése után a következő könyvtárstruktúrát:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-197">After the command completes, you have the following directory structure:</span></span>

    hdinsight-eventhub-example/
        TemperatureMonitor/ - this contains the topology
            resources/
                log4j2.xml - set logging to minimal.
                no-hbase.yaml - topology definition without hbase components.
                with-hbase.yaml - topology definition with hbase components.
            src/main/java/com/microsoft/examples/bolts/
                ParserBolt.java - parses JSON data into tuples
                DashboardBolt.java - sends data over Socket.IO to the web dashboard.
        dashboard/nodejs/ - this is the node.js web dashboard.
        SendEvents/ - utilities to send fake sensor data.

> [!NOTE]
> <span data-ttu-id="fdcb3-198">Ez a dokumentum nem lépjen részletesen az ebben a példában szereplő kódot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-198">This document does not go in to full details of the code included in this example.</span></span> <span data-ttu-id="fdcb3-199">A kód azonban teljes mértékben megjegyzésként.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-199">However, the code is fully commented.</span></span>

<span data-ttu-id="fdcb3-200">Az Event Hubs olvasni a projekt konfigurálásához nyissa meg a `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` fájlt, és az Event Hubs-adatokat hozzáadni a következő sorokat:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-200">To configure the project to read from Event Hub, open the `hdinsight-eventhub-example/TemperatureMonitor/dev.properties` file and add your Event Hub information to the following lines:</span></span>

```bash
eventhub.read.policy.name: your_read_policy_name
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: your_event_hub_name
eventhub.partitions: 2
```

## <a name="compile-and-test-locally"></a><span data-ttu-id="fdcb3-201">Fordítsa le és helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-201">Compile and test locally</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdcb3-202">A topológia helyileg a használatához a Storm fejlesztői környezetben működő.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-202">Using the topology locally requires a working Storm development environment.</span></span> <span data-ttu-id="fdcb3-203">További információkért lásd: [egy Storm-fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-203">For more information, see [Setting up a Storm development environment](http://storm.apache.org/releases/1.1.0/Setting-up-development-environment.html) at Apache.org.</span></span>

> [!WARNING]
> <span data-ttu-id="fdcb3-204">A Windows környezet használatakor jelenhet meg a `java.io.IOException` a jelentés futtatásakor a topológia helyileg.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-204">If you are using a Windows development environment, you may receive a `java.io.IOException` when running the topology locally.</span></span> <span data-ttu-id="fdcb3-205">Ha igen, helyezze át HDInsight a topológia futó.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-205">If so, move on to running the topology on HDInsight.</span></span>

<span data-ttu-id="fdcb3-206">Az irányítópult a topológia eredményének megtekintéséhez és az adatok tárolása az Event Hubs létrehozása előtt nem kell elindítani.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-206">Before testing, you must start the dashboard to view the output of the topology and generate data to store in Event Hub.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fdcb3-207">A HBase ebben a topológiában összetevője nem aktív helyi tesztelése során.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-207">The HBase component of this topology is not active when testing locally.</span></span> <span data-ttu-id="fdcb3-208">A Java API-t a HBase-fürtöt a fürtök tartalmazó Azure virtuális hálózaton kívülről nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-208">The Java API for the HBase cluster cannot be accessed from outside the Azure Virtual Network that contains the clusters.</span></span>

### <a name="start-the-web-application"></a><span data-ttu-id="fdcb3-209">A webalkalmazás elindítása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-209">Start the web application</span></span>

1. <span data-ttu-id="fdcb3-210">Nyisson meg egy parancssort, és módosítsa a könyvtárat a `hdinsight-eventhub-example/dashboard`.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-210">Open a command prompt and change directories to `hdinsight-eventhub-example/dashboard`.</span></span> <span data-ttu-id="fdcb3-211">A következő paranccsal telepítse a függőségeket, a webes alkalmazás szükséges:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-211">Use the following command to install the dependencies needed by the web application:</span></span>
   
    ```bash
    npm install
    ```

2. <span data-ttu-id="fdcb3-212">Az alábbi parancs segítségével indítsa el a webalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-212">Use the following command to start the web application:</span></span>
   
    ```bash
    node server.js
    ```
   
    <span data-ttu-id="fdcb3-213">Az alábbihoz hasonló üzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-213">You see a message similar to the following text:</span></span>
   
        Server listening at port 3000

3. <span data-ttu-id="fdcb3-214">Nyisson meg egy webböngészőt, és írja be `http://localhost:3000/` címként.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-214">Open a web browser and enter `http://localhost:3000/` as the address.</span></span> <span data-ttu-id="fdcb3-215">Az alábbi képen hasonló lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-215">A page similar to the following image is displayed:</span></span>
   
    ![webes irányítópult](./media/hdinsight-storm-sensor-data-analysis/emptydashboard.png)
   
    <span data-ttu-id="fdcb3-217">Hagyja megnyitva a parancssort.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-217">Leave this command prompt open.</span></span> <span data-ttu-id="fdcb3-218">Tesztelés után Ctrl-C használatával állítsa le a webkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-218">After testing, use Ctrl-C to stop the web server.</span></span>

### <a name="generate-data"></a><span data-ttu-id="fdcb3-219">Adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-219">Generate data</span></span>

> [!NOTE]
> <span data-ttu-id="fdcb3-220">A jelen szakaszban szereplő lépéseket használja a Node.js, így bármilyen platformon is használhatók.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-220">The steps in this section use Node.js so that they can be used on any platform.</span></span> <span data-ttu-id="fdcb3-221">Más nyelv, tekintse meg a `SendEvents` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-221">For other language examples, see the `SendEvents` directory.</span></span>

1. <span data-ttu-id="fdcb3-222">Nyisson meg egy új parancssor, rendszerhéjat vagy terminált, és módosítsa a könyvtárat a `hdinsight-eventhub-example/SendEvents/nodejs`.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-222">Open a new prompt, shell, or terminal, and change directories to `hdinsight-eventhub-example/SendEvents/nodejs`.</span></span> <span data-ttu-id="fdcb3-223">A függőségek az alkalmazás által igényelt telepítéséhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-223">To install the dependencies needed by the application, use the following command:</span></span>

    ```bash
    npm install
    ```

2. <span data-ttu-id="fdcb3-224">Nyissa meg a `app.js` fájlt egy szövegszerkesztőben, és adja meg a korábban beszerzett Eseményközpont információkat:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-224">Open the `app.js` file in a text editor and add the Event Hub information you obtained earlier:</span></span>
   
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
   > <span data-ttu-id="fdcb3-225">Ebben a példában feltételezzük, hogy használt `sensordata` az Eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-225">This example assumes that you have used `sensordata` as the name of your Event Hub.</span></span> <span data-ttu-id="fdcb3-226">És hogy `devices` , amelyen a házirend nevét, egy `Send` jogcímek.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-226">And that `devices` as the name of the policy that has a `Send` claim.</span></span>

3. <span data-ttu-id="fdcb3-227">A következő paranccsal helyezze be az új bejegyzések az Event Hubs:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-227">Use the following command to insert new entries in Event Hub:</span></span>
   
    ```bash
    node app.js
    ```
   
    <span data-ttu-id="fdcb3-228">Megjelenik a kimeneti több sornyi Eseményközpont küldött adatok tartalmazzák:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-228">You see several lines of output that contain the data sent to Event Hub:</span></span>
   
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

### <a name="build-and-start-the-topology"></a><span data-ttu-id="fdcb3-229">Hozza létre, és indítsa el a topológia</span><span class="sxs-lookup"><span data-stu-id="fdcb3-229">Build and start the topology</span></span>

1. <span data-ttu-id="fdcb3-230">Nyisson meg egy új parancssort, és módosítsa a könyvtárat a `hdinsight-eventhub-example/TemperatureMonitor`.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-230">Open a new command prompt and change directories to `hdinsight-eventhub-example/TemperatureMonitor`.</span></span> <span data-ttu-id="fdcb3-231">Építsenek, és a topológia csomag, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-231">To build and package the topology, use the following command:</span></span> 

    ```bash
    mvn clean package
    ```

2. <span data-ttu-id="fdcb3-232">A topológia a helyi módjában indul el, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-232">To start the topology in local mode, use the following command:</span></span>

    ```bash
    storm jar target/TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local --filter dev.properties resources/no-hbase.yaml
    ```

    * <span data-ttu-id="fdcb3-233">`--local`a topológia helyi módban indul.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-233">`--local` starts the topology in local mode.</span></span>
    * <span data-ttu-id="fdcb3-234">`--filter`használja a `dev.properties` feltöltéséhez paraméterek topológia meghatározása a fájl.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-234">`--filter` uses the `dev.properties` file to populate parameters in the topology definition.</span></span>
    * <span data-ttu-id="fdcb3-235">`resources/no-hbase.yaml`használja a `no-hbase.yaml` topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-235">`resources/no-hbase.yaml` uses the `no-hbase.yaml` topology definition.</span></span>
 
   <span data-ttu-id="fdcb3-236">Miután elindult, a topológia bejegyzések olvassa be az Eseményközpont, és elküldi azokat az irányítópulton, a helyi számítógépen futnia.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-236">Once started, the topology reads entries from Event Hub, and sends them to the dashboard running on your local machine.</span></span> <span data-ttu-id="fdcb3-237">Sorok jelennek meg a webes irányítópult, az alábbi képen hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-237">You should see lines appear in the web dashboard, similar to the following image:</span></span>
   
    ![Irányítópult adatokkal](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

2. <span data-ttu-id="fdcb3-239">Az irányítópult futása közben, használja a `node app.js` az előző lépéseket követve új adatok küldése az Event Hubs parancsot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-239">While the dashboard is running, use the `node app.js` command from the previous steps to send new data to Event Hubs.</span></span> <span data-ttu-id="fdcb3-240">Hőmérséklet értékek véletlenszerűen jönnek létre, mert a diagramon megjelenítendő nagy változások hőmérséklet frissítenie kell.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-240">Because the temperature values are randomly generated, the graph should update to show large changes in temperature.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fdcb3-241">A kell lennie a **hdinsight – az eventhub-példa/SendEvents/Nodejs** directory használata esetén a `node app.js` parancsot.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-241">You must be in the **hdinsight-eventhub-example/SendEvents/Nodejs** directory when using the `node app.js` command.</span></span>

3. <span data-ttu-id="fdcb3-242">Annak ellenőrzése, hogy az irányítópult frissíti, után állítsa le a Ctrl + C topológiáját.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-242">After verifying that the dashboard updates, stop the topology using Ctrl+C.</span></span> <span data-ttu-id="fdcb3-243">Ctrl + C használatával állítsa le a helyi webkiszolgáló is.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-243">You can use Ctrl+C to stop the local web server also.</span></span>

## <a name="create-a-storm-and-hbase-cluster"></a><span data-ttu-id="fdcb3-244">A Storm és HBase-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-244">Create a Storm and HBase cluster</span></span>

<span data-ttu-id="fdcb3-245">Ezen lépések szakasz használata egy [Azure Resource Manager sablon](../azure-resource-manager/resource-group-template-deploy.md) hozhat létre Azure-beli virtuális hálózatra és a Storm és HBase-fürtöt a virtuális hálózaton.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-245">The steps in this section use an [Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md) to create an Azure Virtual Network and a Storm and HBase cluster on the virtual network.</span></span> <span data-ttu-id="fdcb3-246">A sablon is létrehoz az Azure Web Apps, és telepíti az irányítópult másolatának bele.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-246">The template also creates an Azure Web App and deploys a copy of the dashboard into it.</span></span>

> [!NOTE]
> <span data-ttu-id="fdcb3-247">Egy virtuális hálózatot, hogy a topológia a Storm-fürt futó közvetlenül kommunikálhatnak a HBase Java API-val HBase-fürtöt szolgál.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-247">A virtual network is used so that the topology running on the Storm cluster can directly communicate with the HBase cluster using the HBase Java API.</span></span>

<span data-ttu-id="fdcb3-248">A Resource Manager-sablon, itt a következő nyilvános blobtárolóban található **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-248">The Resource Manager template used in this document is located in a public blob container at **https://hditutorialdata.blob.core.windows.net/armtemplates/create-linux-based-hbase-storm-cluster-in-vnet-3.6.json**.</span></span>

1. <span data-ttu-id="fdcb3-249">A következő gombra kattintva jelentkezzen be az Azure-ba, és nyissa meg a Resource Manager-sablon az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-249">Click the following button to sign in to Azure and open the Resource Manager template in the Azure portal.</span></span>
   
    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fhditutorialdata.blob.core.windows.net%2Farmtemplates%2Fcreate-linux-based-hbase-storm-cluster-in-vnet-3.6.json" target="_blank"><img src="./media/hdinsight-storm-sensor-data-analysis/deploy-to-azure.png" alt="Deploy to Azure"></a>

2. <span data-ttu-id="fdcb3-250">Az a **egyéni telepítési** területen adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-250">From the **Custom deployment** section, enter the following values:</span></span>
   
    ![A HDInsight-paraméterek](./media/hdinsight-storm-sensor-data-analysis/parameters.png)
   
   * <span data-ttu-id="fdcb3-252">**Fürt neve kiinduló**: Ez az érték szerepe alapnevét a Storm és HBase fürtök.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-252">**Base Cluster Name**: This value is used as the base name for the Storm and HBase clusters.</span></span> <span data-ttu-id="fdcb3-253">Ha például **abc** hoz létre egy Storm-fürt nevű **storm-abc** és nevű HBase-fürtöt **hbase-abc**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-253">For example, entering **abc** creates a Storm cluster named **storm-abc** and an HBase cluster named **hbase-abc**.</span></span>
   * <span data-ttu-id="fdcb3-254">**A fürt bejelentkezési felhasználónevét**: A rendszergazda felhasználóneve a Storm és HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-254">**Cluster Login User Name**: The admin user name for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="fdcb3-255">**A fürt bejelentkezési jelszó**: a Storm és HBase fürt rendszergazdai jelszóval.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-255">**Cluster Login Password**: The admin user password for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="fdcb3-256">**SSH-felhasználónév**: az SSH-felhasználó a Storm és HBase fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-256">**SSH User Name**: The SSH user to create for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="fdcb3-257">**SSH-jelszónak**: a Storm és HBase fürt SSH-felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-257">**SSH Password**: The password for the SSH user for the Storm and HBase clusters.</span></span>
   * <span data-ttu-id="fdcb3-258">**Hely**: A régiót, amelyben a fürtök jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-258">**Location**: The region that the clusters are created in.</span></span>
     
     <span data-ttu-id="fdcb3-259">Kattintson az **OK** gombra a paraméterek mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-259">Click **OK** to save the parameters.</span></span>

3. <span data-ttu-id="fdcb3-260">Használja a **alapjai** szakasz hozzon létre egy erőforráscsoportot, vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-260">Use the **Basics** section to create a resource group or select an existing one.</span></span>
4. <span data-ttu-id="fdcb3-261">Az a **erőforráscsoport helye** legördülő menüben válassza ki kijelölt ugyanarra a helyre, ha Ön a **hely** paramétere a **beállítások** szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-261">In the **Resource group location** dropdown menu, select the same location as you selected for the **Location** parameter in the **Settings** section.</span></span>
5. <span data-ttu-id="fdcb3-262">Olvassa el a használati feltételeket, majd válassza ki **elfogadom a feltételeket és a fenti feltételek**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-262">Read the terms and conditions, and then select **I agree to the terms and conditions stated above**.</span></span>
6. <span data-ttu-id="fdcb3-263">Végül ellenőrizze **rögzítés az irányítópulton** majd **beszerzési**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-263">Finally, check **Pin to dashboard** and then select **Purchase**.</span></span> <span data-ttu-id="fdcb3-264">A fürt létrehozása nagyjából 20 percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-264">It takes about 20 minutes to create the clusters.</span></span>

<span data-ttu-id="fdcb3-265">Az erőforrások létrehozása után, az erőforráscsoport információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-265">Once the resources have been created, information about the resource group is displayed.</span></span>

![A virtuális hálózat és a fürtök tartozó erőforráscsoport](./media/hdinsight-storm-sensor-data-analysis/groupblade.png)

> [!IMPORTANT]
> <span data-ttu-id="fdcb3-267">Figyelje meg, hogy a HDInsight-fürtök neve **storm-BASENAME** és **hbase-BASENAME**, ahol BASENAME a sablonhoz megadott név.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-267">Notice that the names of the HDInsight clusters are **storm-BASENAME** and **hbase-BASENAME**, where BASENAME is the name you provided to the template.</span></span> <span data-ttu-id="fdcb3-268">Ezeket a neveket használni egy későbbi lépésben a fürtök való kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-268">You use these names in a later step when connecting to the clusters.</span></span> <span data-ttu-id="fdcb3-269">Is vegye figyelembe, hogy az irányítópult-hely nevével **basename-irányítópult**.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-269">Also note that the name of the dashboard site is **basename-dashboard**.</span></span> <span data-ttu-id="fdcb3-270">Ez az érték használható a dokumentum későbbi szakaszában.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-270">This value is used later in this document.</span></span>

## <a name="configure-the-dashboard-bolt"></a><span data-ttu-id="fdcb3-271">Az irányítópult bolt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-271">Configure the Dashboard bolt</span></span>

<span data-ttu-id="fdcb3-272">Az irányítópult webes alkalmazás központilag telepített adatok küldését, módosítania kell a következő sort a a `dev.properties`fájlt:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-272">To send data to the dashboard deployed as a web app, you must modify the following line in the `dev.properties`file:</span></span>

```yaml
dashboard.uri: http://localhost:3000
```

<span data-ttu-id="fdcb3-273">Változás `http://localhost:3000` való `http://BASENAME-dashboard.azurewebsites.net` , és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-273">Change `http://localhost:3000` to `http://BASENAME-dashboard.azurewebsites.net` and save the file.</span></span> <span data-ttu-id="fdcb3-274">Cserélje le **BASENAME** az Alap az előző lépésben megadott névvel.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-274">Replace **BASENAME** with the base name you provided in the previous step.</span></span> <span data-ttu-id="fdcb3-275">Válassza ki az irányítópultot, és tekintse meg az URL-címet korábban létrehozott erőforráscsoportot is használható.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-275">You can also use the resource group created previously to select the dashboard and view the URL.</span></span>

## <a name="create-the-hbase-table"></a><span data-ttu-id="fdcb3-276">A HBase tábla létrehozása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-276">Create the HBase table</span></span>

<span data-ttu-id="fdcb3-277">Adatok tárolása a HBase, azt először létre kell hozni egy tábla.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-277">To store data in HBase, we must first create a table.</span></span> <span data-ttu-id="fdcb3-278">Előzetes létrehozása az erőforrásokat, amelyek a Storm kell írni, és az erőforráshoz, a Storm létrehozására tett kísérlet, topológia ugyanaz az erőforrás létrehozásának megkísérlése több példánya is okozhat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-278">Pre-create resources that Storm needs to write to, as trying to create resources from inside a Storm topology can result in multiple instances trying to create the same resource.</span></span> <span data-ttu-id="fdcb3-279">A topológia kívüli erőforrások létrehozását, és olvasási/írási és az elemzések Storm használatát.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-279">Create the resources outside the topology and use Storm for reading/writing and analytics.</span></span>

1. <span data-ttu-id="fdcb3-280">Az SSH használata a HBase-fürtöt az SSH-felhasználó és a fürt létrehozása során a sablonba megadott jelszóval való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-280">Use SSH to connect to the HBase cluster using the SSH user and password you supplied to the template during cluster creation.</span></span> <span data-ttu-id="fdcb3-281">Például, ha csatlakozik a `ssh` parancsot, akkor használja a következő szintaxist:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-281">For example, if connecting using the `ssh` command, you would use the following syntax:</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```
   
    <span data-ttu-id="fdcb3-282">Cserélje le `sshuser` az a fürt létrehozásakor megadott SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-282">Replace `sshuser` with the SSH user name you provided when creating the cluster.</span></span> <span data-ttu-id="fdcb3-283">Cserélje le `clustername` a HBase fürt nevéhez.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-283">Replace `clustername` with the HBase cluster name.</span></span>

2. <span data-ttu-id="fdcb3-284">Az SSH-munkamenetből indítsa el a HBase rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-284">From the SSH session, start the HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="fdcb3-285">Miután a rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-285">Once the shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="fdcb3-286">A HBase rendszerhéjból adja meg a érzékelő adatokat tároló tábla létrehozása a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-286">From the HBase shell, enter the following command to create a table to store the sensor data:</span></span>
   
    ```hbase
    create 'SensorData', 'cf'
    ```

4. <span data-ttu-id="fdcb3-287">Győződjön meg arról, hogy létrejött-e a tábla a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-287">Verify that the table has been created by using the following command:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="fdcb3-288">Ez információkat ad vissza a következőhöz hasonló, amely azt jelzi, hogy nincsenek-e 0 sorokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-288">This returns information similar to the following example, indicating that there are 0 rows in the table.</span></span>
   
        ROW                   COLUMN+CELL                                       0 row(s) in 0.1900 seconds

5. <span data-ttu-id="fdcb3-289">Adja meg `exit` való kilépéshez a HBase rendszerhéjból:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-289">Enter `exit` to exit the HBase shell:</span></span>

## <a name="configure-the-hbase-bolt"></a><span data-ttu-id="fdcb3-290">A HBase bolt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdcb3-290">Configure the HBase bolt</span></span>

<span data-ttu-id="fdcb3-291">A Storm-fürtök írni a HBase, a HBase bolt kell megadnia a konfigurációs adatait a HBase fürt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-291">To write to HBase from the Storm cluster, you must provide the HBase bolt with the configuration details of your HBase cluster.</span></span>

1. <span data-ttu-id="fdcb3-292">Az alábbi példák segítségével a Zookeeper a HBase fürt kvórumát beolvasása:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-292">Use one of the following examples to retrieve the Zookeeper quorum for your HBase cluster:</span></span>

    ```bash
    CLUSTERNAME='your_HDInsight_cluster_name'
    curl -u admin -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HBASE/components/HBASE_MASTER" | jq '.metrics.hbase.master.ZookeeperQuorum'
    ```

    > [!NOTE]
    > <span data-ttu-id="fdcb3-293">Cserélje le a `your_HDInsight_cluster_name` kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-293">Replace `your_HDInsight_cluster_name` with the name of your HDInsight cluster.</span></span> <span data-ttu-id="fdcb3-294">Telepítéséről további információt a `jq` segédprogram, lásd: [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-294">For more information on installing the `jq` utility, see [https://stedolan.github.io/jq/](https://stedolan.github.io/jq/).</span></span>
    >
    > <span data-ttu-id="fdcb3-295">Amikor a rendszer kéri, adja meg a jelszót a HDInsight-rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-295">When prompted, enter the password for the HDInsight admin login.</span></span>

    ```powershell
    $clusterName = 'your_HDInsight_cluster_name`
    $creds = Get-Credential -UserName "admin" -Message "Enter the HDInsight login"
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HBASE/components/HBASE_MASTER" -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.metrics.hbase.master.ZookeeperQuorum
    ```

    > [!NOTE]
    > <span data-ttu-id="fdcb3-296">Cserélje le a(z) your_HDInsight_cluster_name a HDInsight-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-296">Replace \`your_HDInsight_cluster_name with the name of your HDInsight cluster.</span></span> <span data-ttu-id="fdcb3-297">Amikor a rendszer kéri, adja meg a jelszót a HDInsight-rendszergazdai bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-297">When prompted, enter the password for the HDInsight admin login.</span></span>
    >
    > <span data-ttu-id="fdcb3-298">Ebben a példában az Azure PowerShell szükséges.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-298">This example requires Azure PowerShell.</span></span> <span data-ttu-id="fdcb3-299">További információ az Azure PowerShell használatával, lásd: [Ismerkedés az Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span><span class="sxs-lookup"><span data-stu-id="fdcb3-299">For more information on using Azure PowerShell, see [Get started with Azure PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/Getting-Started-with-Windows-PowerShell?view=powershell-6)</span></span>

    <span data-ttu-id="fdcb3-300">Ezek a példák által visszaadott adatok hasonlít a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-300">The information returned by these examples is similar to the following text:</span></span>

    `zk2-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk0-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181,zk3-hbase.mf0yeg255m4ubit1auvj1tutvh.ex.internal.cloudapp.net:2181`

    <span data-ttu-id="fdcb3-301">Ezt az információt használják a Storm kommunikálni a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-301">This information is used by Storm to communicate with the HBase cluster.</span></span>

2. <span data-ttu-id="fdcb3-302">Módosítsa a `dev.properties` fájlt, és a Zookeeper kvórum-adatokat hozzáadni a következő sort:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-302">Modify the `dev.properties` file and add the Zookeeper quorum information to the following line:</span></span>

    ```yaml
    hbase.zookeeper.quorum: your_hbase_quorum
    ```

## <a name="build-package-and-deploy-the-solution-to-hdinsight"></a><span data-ttu-id="fdcb3-303">Hozza létre, csomagot, és a HDInsight a megoldás telepítése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-303">Build, package, and deploy the solution to HDInsight</span></span>

<span data-ttu-id="fdcb3-304">A fejlesztői környezetben tegye a következőket a Storm-topológia a storm-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-304">In your development environment, use the following steps to deploy the Storm topology to the storm cluster.</span></span>

1. <span data-ttu-id="fdcb3-305">Az a `TemperatureMonitor` directory, a következő parancsot egy új build végrehajtani, és hozzon létre egy JAR csomag a projektből használható:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-305">From the `TemperatureMonitor` directory, use the following command to perform a new build and create a JAR package from your project:</span></span>
   
        mvn clean package
   
    <span data-ttu-id="fdcb3-306">Ez a parancs létrehoz egy nevű fájlt `TemperatureMonitor-1.0-SNAPSHOT.jar in the `cél "mappában található a projekthez.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-306">This command creates a file named `TemperatureMonitor-1.0-SNAPSHOT.jar in the `target\` directory of your project.</span></span>

2. <span data-ttu-id="fdcb3-307">Szolgáltatáskapcsolódási pont használatával töltse fel a `TemperatureMonitor-1.0-SNAPSHOT.jar` és `dev.properties` a Storm-fürt fájlokat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-307">Use scp to upload the `TemperatureMonitor-1.0-SNAPSHOT.jar` and `dev.properties` files to your Storm cluster.</span></span> <span data-ttu-id="fdcb3-308">Az alábbi példában cserélje le `sshuser` a fürt létrehozásakor megadott SSH-felhasználói és `clustername` a Storm-fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-308">In the following example, replace `sshuser` with the SSH user you provided when creating the cluster, and `clustername` with the name of your Storm cluster.</span></span> <span data-ttu-id="fdcb3-309">Amikor a rendszer kéri, adja meg az SSH-felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-309">When prompted, enter the password for the SSH user.</span></span>
   
    ```bash
    scp target/TemperatureMonitor-1.0-SNAPSHOT.jar dev.properties sshuser@clustername-ssh.azurehdinsight.net:
    ```

   > [!NOTE]
   > <span data-ttu-id="fdcb3-310">A fájlok feltöltéséhez több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-310">It may take several minutes to upload the files.</span></span>

    <span data-ttu-id="fdcb3-311">További információk az a `scp` és `ssh` parancsok a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](./hdinsight-hadoop-linux-use-ssh-unix.md)</span><span class="sxs-lookup"><span data-stu-id="fdcb3-311">For more information on using the `scp` and `ssh` commands with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md)</span></span>

3. <span data-ttu-id="fdcb3-312">A fájl feltöltése után csatlakozzon a Storm fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-312">Once the file has been uploaded, connect to the Storm cluster using SSH.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fdcb3-313">Cserélje le `sshuser` az SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-313">Replace `sshuser` with the SSH user name.</span></span> <span data-ttu-id="fdcb3-314">Cserélje le `clustername` a Storm-fürt nevű.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-314">Replace `clustername` with the Storm cluster name.</span></span>

4. <span data-ttu-id="fdcb3-315">Indítsa el a topológia, használja az SSH-munkamenetet a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-315">To start the topology, use the following command from the SSH session:</span></span>
   
    ```bash
    storm jar TemperatureMonitor-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote --filter dev.properties -R /with-hbase.yaml
    ```

    * <span data-ttu-id="fdcb3-316">`--remote`a topológia a Nimbus szolgáltatás, amely elküldi a fürt felügyelő csomópontja küldi el.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-316">`--remote` submits the topology to the Nimbus service, which distributes it to the supervisor nodes in the cluster.</span></span>
    * <span data-ttu-id="fdcb3-317">`--filter`használja a `dev.properties` feltöltéséhez paraméterek topológia meghatározása a fájl.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-317">`--filter` uses the `dev.properties` file to populate parameters in the topology definition.</span></span>
    * <span data-ttu-id="fdcb3-318">`-R /with-hbase.yaml`használja a `with-hbase.yaml` topológia a csomagban található.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-318">`-R /with-hbase.yaml` uses the `with-hbase.yaml` topology included in the package.</span></span>

5. <span data-ttu-id="fdcb3-319">Miután elindult a topológia, egy böngészőben nyissa meg a webhely, az Azure-on közzétett, majd használja a `node app.js` szeretnék adatokat küldeni a Eseményközpont parancs.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-319">After the topology has started, open a browser to the website you published on Azure, then use the `node app.js` command to send data to Event Hub.</span></span> <span data-ttu-id="fdcb3-320">Meg kell jelennie a webes irányítópult frissül, és megjeleníti az adatokat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-320">You should see the web dashboard update to display the information.</span></span>
   
    ![irányítópult](./media/hdinsight-storm-sensor-data-analysis/datadashboard.png)

## <a name="view-hbase-data"></a><span data-ttu-id="fdcb3-322">A HBase-adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-322">View HBase data</span></span>

<span data-ttu-id="fdcb3-323">Az alábbi lépések segítségével HBase csatlakozhat, és győződjön meg arról, hogy az adatok a táblázatban van írva:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-323">Use the following steps to connect to HBase and verify that the data has been written to the table:</span></span>

1. <span data-ttu-id="fdcb3-324">SSH használatával csatlakozhat a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-324">Use SSH to connect to the HBase cluster.</span></span>
   
    ```bash
    ssh sshuser@clustername-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fdcb3-325">Cserélje le `sshuser` az SSH-felhasználónév.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-325">Replace `sshuser` with the SSH user name.</span></span> <span data-ttu-id="fdcb3-326">Cserélje le `clustername` a HBase fürt nevéhez.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-326">Replace `clustername` with the HBase cluster name.</span></span>

2. <span data-ttu-id="fdcb3-327">Az SSH-munkamenetből indítsa el a HBase rendszerhéjból.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-327">From the SSH session, start the HBase shell.</span></span>
   
    ```bash
    hbase shell
    ```
   
    <span data-ttu-id="fdcb3-328">Miután a rendszerhéj be van töltve, megjelenik egy `hbase(main):001:0>` kérdés.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-328">Once the shell has loaded, you see an `hbase(main):001:0>` prompt.</span></span>

3. <span data-ttu-id="fdcb3-329">Nézet sorokat a táblázatból:</span><span class="sxs-lookup"><span data-stu-id="fdcb3-329">View rows from the table:</span></span>
   
    ```hbase
    scan 'SensorData'
    ```
   
    <span data-ttu-id="fdcb3-330">Ez a parancs visszaadja a információ az alábbihoz hasonló, jelezve, hogy nincs-e adatokat a táblában.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-330">This command returns information similar to the following text, indicating that there is data in the table.</span></span>
   
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
   > <span data-ttu-id="fdcb3-331">A vizsgálati művelet legfeljebb 10 sorokat a tábla adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-331">This scan operation returns a maximum of 10 rows from the table.</span></span>

## <a name="delete-your-clusters"></a><span data-ttu-id="fdcb3-332">A fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="fdcb3-332">Delete your clusters</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="fdcb3-333">A fürtök, a tároló és a webalkalmazást egy időben törléséhez törölje a csoportot, amely tartalmazza azokat.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-333">To delete the clusters, storage, and web app at one time, delete the resource group that contains them.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdcb3-334">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdcb3-334">Next steps</span></span>

<span data-ttu-id="fdcb3-335">A HDInsight Storm-topológiák további példákért lásd [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md)</span><span class="sxs-lookup"><span data-stu-id="fdcb3-335">For more examples of Storm topologies with HDInsight, see [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md)</span></span>

<span data-ttu-id="fdcb3-336">Apache Storm kapcsolatos további információkért tekintse meg a [alatt futó Apache Storm](https://storm.incubator.apache.org/) hely.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-336">For more information about Apache Storm, see the [Apache Storm](https://storm.incubator.apache.org/) site.</span></span>

<span data-ttu-id="fdcb3-337">A HDInsight HBase kapcsolatos további információkért tekintse meg a [HBase a HDInsight áttekintése](hdinsight-hbase-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-337">For more information about HBase on HDInsight, see the [HBase with HDInsight Overview](hdinsight-hbase-overview.md).</span></span>

<span data-ttu-id="fdcb3-338">Socket.io kapcsolatos további információkért tekintse meg a [socket.io](http://socket.io/) hely.</span><span class="sxs-lookup"><span data-stu-id="fdcb3-338">For more information about Socket.io, see the [socket.io](http://socket.io/) site.</span></span>

<span data-ttu-id="fdcb3-339">D3.js kapcsolatos további információkért lásd: [D3.js - adatokat tartalmazó dokumentumok vezérelt](http://d3js.org/).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-339">For more information about D3.js, see [D3.js - Data Driven Documents](http://d3js.org/).</span></span>

<span data-ttu-id="fdcb3-340">A Java topológiák létrehozásával kapcsolatos további információkért lásd: [HDInsight alatt futó Apache Storm topológiák fejlesztése Java](hdinsight-storm-develop-java-topology.md).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-340">For information about creating topologies in Java, see [Develop Java topologies for Apache Storm on HDInsight](hdinsight-storm-develop-java-topology.md).</span></span>

<span data-ttu-id="fdcb3-341">A .NET topológiák létrehozásával kapcsolatos további információkért lásd: [fejlesztésére C#-topológiák futó Apache stormra a Visual Studio használatával HDInsight](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span><span class="sxs-lookup"><span data-stu-id="fdcb3-341">For information about creating topologies in .NET, see [Develop C# topologies for Apache Storm on HDInsight using Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md).</span></span>

[azure-portal]: https://portal.azure.com
