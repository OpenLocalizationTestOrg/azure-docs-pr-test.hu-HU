---
title: "Apache Spark streamelési az Event Hubs az Azure HDInsight használata |} Microsoft Docs"
description: "Build Apache Spark streaming minta adatfolyam küldeni az Azure Event Hubs és a HDInsight Spark-fürt scala-alkalmazások segítségével a ezeket az eseményeket fogadni."
keywords: "Apache spark streaming, spark streamelési, spark minta, apache spark streamelési, például az esemény hub azure-minta, spark-minta"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 68894e75-3ffa-47bd-8982-96cdad38b7d0
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: nitinme
ms.openlocfilehash: 175a2ad70b1f554d05846eb62fb685d4f259af7e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="e5b2d-104">Apache Spark streaming: a HDInsight fürt adatfeldolgozásra Spark az Azure Event hubs</span><span class="sxs-lookup"><span data-stu-id="e5b2d-104">Apache Spark streaming: Process data from Azure Event Hubs with Spark cluster on HDInsight</span></span>

<span data-ttu-id="e5b2d-105">Ebben a cikkben hozzon létre egy Apache Spark streaming mintát, amely a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-105">In this article, you create an Apache Spark streaming sample that involves the following steps:</span></span>

1. <span data-ttu-id="e5b2d-106">Egy önálló alkalmazás segítségével üzenetek betöltési Azure eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-106">You use a standalone application to ingest messages into an Azure Event Hub.</span></span>

2. <span data-ttu-id="e5b2d-107">A két különböző szempontok, akkor a üzeneteket beolvasni az Event Hubs a valós idejű Azure HDInsight Spark-fürtön futó alkalmazást használ.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-107">With two different approaches, you retrieve the messages from Event Hub in real-time using an application running in Spark cluster on Azure HDInsight.</span></span>

3. <span data-ttu-id="e5b2d-108">Build a streaming adatok különböző tárolórendszerek megőrzéséhez elemzőfolyamatokat, vagy mélyebb betekintés az adatok menet közben.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-108">You build streaming analytic pipelines to persist data to different storage systems, or get insights from data on the fly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e5b2d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e5b2d-109">Prerequisites</span></span>

* <span data-ttu-id="e5b2d-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-110">An Azure subscription.</span></span> <span data-ttu-id="e5b2d-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="e5b2d-112">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="e5b2d-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="spark-streaming-concepts"></a><span data-ttu-id="e5b2d-114">Spark Streaming fogalmak</span><span class="sxs-lookup"><span data-stu-id="e5b2d-114">Spark Streaming concepts</span></span>

<span data-ttu-id="e5b2d-115">A Spark streamelési részletes ismertetése, [Apache Spark streaming áttekintése](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-115">For a detailed explanation of Spark streaming, see [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span></span> <span data-ttu-id="e5b2d-116">HDInsight biztosítható a az Azure Spark-fürt azonos adatfolyam-továbbítási funkciókat.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-116">HDInsight brings the same streaming features to a Spark cluster on Azure.</span></span>  

## <a name="what-does-this-solution-do"></a><span data-ttu-id="e5b2d-117">Mire ehhez a megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="e5b2d-117">What does this solution do?</span></span>

<span data-ttu-id="e5b2d-118">Ebben a cikkben egy Spark streaming példa létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-118">In this article, to create a Spark streaming example, perform the following steps:</span></span>

1. <span data-ttu-id="e5b2d-119">Hozzon létre egy Azure Event hubs Eseményközpontot, akik szintén megkapják az események adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-119">Create an Azure Event Hub that will receive a stream of events.</span></span>

2. <span data-ttu-id="e5b2d-120">Futtassa egy helyi önálló alkalmazás, amely eseményeket hoz létre, és az Azure Event hubs leküldéses értesítések azt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-120">Run a local standalone application that generates events and pushes it to the Azure Event Hub.</span></span> <span data-ttu-id="e5b2d-121">A mintaalkalmazás, amely ezt a közzétett [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-121">The sample application that does this is published at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span>

3. <span data-ttu-id="e5b2d-122">Távolról futtatni egy adatfolyam-továbbítási alkalmazást egy Spark-fürt, amely az Azure Event Hubs olvassa be az esemény streamelését és végezhetnek különböző adatfeldolgozási /.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-122">Run a streaming application remotely on a Spark cluster that reads streaming events from Azure Event Hub and perform various data processing/analysis.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="e5b2d-123">Hozzon létre egy Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="e5b2d-123">Create an Azure Event Hub</span></span>

1. <span data-ttu-id="e5b2d-124">Jelentkezzen be a [Azure Portal](https://ms.portal.azure.com), és kattintson a **új** , a képernyő bal felső.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-124">Log on to the [Azure Portal](https://ms.portal.azure.com), and click **New** at the top left of the screen.</span></span>

2. <span data-ttu-id="e5b2d-125">Kattintson az **Eszközök internetes hálózata**, majd az **Event Hubs** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-125">Click **Internet of Things**, then click **Event Hubs**.</span></span>

    <span data-ttu-id="e5b2d-126">![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark streamelési példa eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-126">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")</span></span>

3. <span data-ttu-id="e5b2d-127">A **Névtér létrehozása** panelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-127">In the **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="e5b2d-128">Válassza ki a tarifacsomag (alapszintű vagy Standard).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-128">choose the pricing tier (Basic or Standard).</span></span> <span data-ttu-id="e5b2d-129">Valamint válassza ki azt az Azure-előfizetést, erőforráscsoportot és helyet, amellyel az erőforrást létre kívánja hozni.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-129">Also, choose an Azure subscription, resource group, and location in which to create the resource.</span></span> <span data-ttu-id="e5b2d-130">A névtér létrehozásához kattintson a **Létrehozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-130">Click **Create** to create the namespace.</span></span>

      <span data-ttu-id="e5b2d-131">![Adjon meg a Spark streamelési példa event hub nevet](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "adja meg az eseményközpont neveként a Spark streamelési – példa")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-131">![Provide an event hub name for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")</span></span>

    > [!NOTE]
    > <span data-ttu-id="e5b2d-132">Ki kell jelölni a azonos **hely** , az Apache Spark-fürt hdinsightban késés és a költségek csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-132">You should select the same **Location** as your Apache Spark cluster in HDInsight to reduce latency and costs.</span></span>
    >
    >

4. <span data-ttu-id="e5b2d-133">Az Event Hubs névtérlistájában kattintson az újonnan létrehozott névtérre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-133">In the Event Hubs namespace list, click the newly-created namespace.</span></span>      


5. <span data-ttu-id="e5b2d-134">A névtér paneljén kattintson **Event Hubs**, és kattintson a **+ Eseményközpont** egy új Eseményközpont létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-134">In the namespace blade, click **Event Hubs**, and then click **+ Event Hub** to create a new Event Hub.</span></span>
   
    <span data-ttu-id="e5b2d-135">![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Spark streamelési példa eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-135">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Create event hub for Spark streaming example")</span></span>

6. <span data-ttu-id="e5b2d-136">Adjon meg egy nevet az Eseményközpont, állítsa be a partíciók száma 10 és üzenet megőrzési 1.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-136">Type a name for your Event Hub, set the partition count to 10, and message retention to 1.</span></span> <span data-ttu-id="e5b2d-137">Jelenleg nem archiválni az üzenetek ebben a megoldásban, a többi alapértelmezett maradjon, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-137">We are not archiving the messages in this solution so you can leave the rest as default, and then click **Create**.</span></span>
   
    <span data-ttu-id="e5b2d-138">![Adja meg a Spark streaming példa event hub-adatok](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark streamelési példa event hub-adatok megadása")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-138">![Provide event hub details for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")</span></span>

7. <span data-ttu-id="e5b2d-139">Az újonnan létrehozott Eseményközpont, az Event Hubs panel szerepel.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-139">The newly created Event Hub is listed in the Event Hub blade.</span></span>
    
     <span data-ttu-id="e5b2d-140">![Az Event Hubs szeretné megtekinteni a Spark streaming példa](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "nézet Eseményközpont a Spark streaming – példa")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-140">![View Event Hub for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub for the Spark streaming example")</span></span>

8. <span data-ttu-id="e5b2d-141">Térjen vissza a névtér panelre (nem az adott eseményközpont panelére), kattintson a **Megosztott elérési házirendek**, majd a **RootManageSharedAccessKey** elemre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-141">Back in the namespace blade (not the specific Event Hub blade), click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
     <span data-ttu-id="e5b2d-142">![Az Event Hubs házirendjeinek beállítása a Spark streaming példa](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "állítsa be az Event Hubs házirendeket a Spark streaming – példa")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-142">![Set Event Hub policies for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for the Spark streaming example")</span></span>

9. <span data-ttu-id="e5b2d-143">A Másolás gombra kattintva másolja a **RootManageSharedAccessKey** elsődleges kulcs és a kapcsolati karakterláncot a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-143">Click the copy button to copy the **RootManageSharedAccessKey** primary key and connection string to the clipboard.</span></span> <span data-ttu-id="e5b2d-144">Mentse őket az oktatóanyag későbbi részében használni.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-144">Save these to use later in the tutorial.</span></span>
    
     <span data-ttu-id="e5b2d-145">![Az Event Hubs házirend kulcsok megtekintéséhez a Spark streaming példa](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "a Spark streamelési például kulcsok megtekintése az Event Hubs házirend")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-145">![View Event Hub policy keys for the Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for the Spark streaming example")</span></span>

## <a name="send-messages-to-azure-event-hub-using-a-sample-scala-application"></a><span data-ttu-id="e5b2d-146">Üzenetek küldése az Azure Event Hubs egy mintaalkalmazás Scala használatával</span><span class="sxs-lookup"><span data-stu-id="e5b2d-146">Send messages to Azure Event Hub using a sample Scala application</span></span>

<span data-ttu-id="e5b2d-147">Ebben a szakaszban egy különálló helyi Scala alkalmazás, amely események adatfolyam hoz létre, és elküldi azt korábban létrehozott Azure Event Hubs használja.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-147">In this section you use a standalone local Scala application that generates a stream of events and sends it to Azure Event Hub that you created earlier.</span></span> <span data-ttu-id="e5b2d-148">Ez az alkalmazás érhető el a Githubon: [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-148">This application is available on GitHub at [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span></span> <span data-ttu-id="e5b2d-149">Itt a lépések azt feltételezik, hogy Ön rendelkezik már ágazik el a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-149">The steps here assume that you have already forked this GitHub repository.</span></span>

1. <span data-ttu-id="e5b2d-150">Győződjön meg arról, hogy ezt az alkalmazást futtató számítógépen a következő.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-150">Make sure you have the following installed on the computer where you run this application.</span></span>

    * <span data-ttu-id="e5b2d-151">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-151">Oracle Java Development kit.</span></span> <span data-ttu-id="e5b2d-152">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-152">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
    * <span data-ttu-id="e5b2d-153">Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-153">Apache Maven.</span></span> <span data-ttu-id="e5b2d-154">Letöltheti a [Itt](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-154">You can download it from [here](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="e5b2d-155">Útmutatás Maven telepítendő érhető el [Itt](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-155">Instructions to install Maven are available [here](https://maven.apache.org/install.html).</span></span>

2. <span data-ttu-id="e5b2d-156">Nyisson meg egy parancssort, és keresse meg a helyet, a GitHub-tárház Scala mintaalkalmazás és futni építenie az alkalmazást a következő parancsot a klónozott.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-156">Open a command prompt and navigate to the location you cloned the GitHub repo for the sample Scala application and run the following command to build the application.</span></span>

        mvn package

3. <span data-ttu-id="e5b2d-157">Az alkalmazás a kimeneti jar **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, alatt létrejön **/target** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-157">The output jar for the application, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, is created under **/target** directory.</span></span> <span data-ttu-id="e5b2d-158">Ez a cikk későbbi részében JAR segítségével tesztelheti a teljes megoldás.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-158">You use this JAR later in this article to test the complete solution.</span></span>

## <a name="create-application-to-receive-messages-from-event-hub-into-a-spark-cluster"></a><span data-ttu-id="e5b2d-159">Üzenetek fogadása az Event Hubs egy Spark-fürt az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-159">Create application to receive messages from Event Hub into a Spark cluster</span></span> 

<span data-ttu-id="e5b2d-160">Spark Streaming és az Azure Event Hubs, címzett-alapú kapcsolat és közvetlen DStream-alapú kapcsolat két módszer van.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-160">We have two approaches to connect Spark Streaming and Azure Event Hubs, Receiver-based connection and Direct-DStream-based connection.</span></span> <span data-ttu-id="e5b2d-161">Jan 2017, a közvetlen DStream-alapú megjelent a 2.0.3 kiadásában.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-161">Direct-DStream-based is introduced on Jan of 2017, in the 2.0.3 release.</span></span> <span data-ttu-id="e5b2d-162">Lecseréli az eredeti fogadó-alapú kapcsolatot, mert az több performant feltételezett és erőforrás-hatékony.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-162">It is supposed to replace the original receiver-based connection as it is more performant and resource-efficient.</span></span> <span data-ttu-id="e5b2d-163">További részletek találhatók [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-163">More details found in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span></span> <span data-ttu-id="e5b2d-164">Közvetlen DStream csak a Spark 2.0 + támogatja.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-164">Direct DStream only supports Spark 2.0+.</span></span>

### <a name="build-applications-with-the-dependency-to-spark-eventhubs-connector"></a><span data-ttu-id="e5b2d-165">A spark-eventhubs összekötő a függőséggel rendelkező alkalmazások</span><span class="sxs-lookup"><span data-stu-id="e5b2d-165">Build applications with the dependency to spark-eventhubs connector</span></span>

<span data-ttu-id="e5b2d-166">Azt is közzétesz a Spark-EventHubs átmeneti verzióját a Githubon.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-166">We will also publish the staging version of Spark-EventHubs in GitHub.</span></span> <span data-ttu-id="e5b2d-167">A Spark-EventHubs átmeneti verzióját használja, az első lépés GitHub jelzi a forrás-tárház, a következő bejegyzés hozzáadásával pom.xml:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-167">To use the staging version of Spark-EventHubs, the first step is to indicate GitHub as the source repo by adding the following entry to pom.xml:</span></span>

```xml
<repository>
      <id>spark-eventhubs</id>
      <url>https://raw.github.com/hdinsight/spark-eventhubs/maven-repo/</url>
      <snapshots>
        <enabled>true</enabled>
        <updatePolicy>always</updatePolicy>
      </snapshots>
</repository>
```

<span data-ttu-id="e5b2d-168">A következő függőségi majd hozzá a projekthez az előzetes verzióra érvénybe.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-168">You can then add the following dependency to your project to take the pre-released version.</span></span>

<span data-ttu-id="e5b2d-169">Maven-függőség</span><span class="sxs-lookup"><span data-stu-id="e5b2d-169">Maven Dependency</span></span>

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

<span data-ttu-id="e5b2d-170">SBT függőség</span><span class="sxs-lookup"><span data-stu-id="e5b2d-170">SBT Dependency</span></span>

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a><span data-ttu-id="e5b2d-171">Közvetlen DStream kapcsolat</span><span class="sxs-lookup"><span data-stu-id="e5b2d-171">Direct DStream Connection</span></span>

<span data-ttu-id="e5b2d-172">A letölthető egy előre elkészített jar-fájlt tartalmazó példák segítségével közvetlen DStream [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-172">A pre-built jar file containing examples using Direct DStream can be downloaded in [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span></span>

<span data-ttu-id="e5b2d-173">A jar-fájlt tartalmaz három példák, amelyek a forráskód érhetők el [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-173">The jar file contains three examples whose source code are available at [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span></span>

<span data-ttu-id="e5b2d-174">Véve [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) példa:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-174">Taking [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) as an example:</span></span>

```scala
private def createStreamingContext(
  sparkCheckpointDir: String,
  batchDuration: Int,
  namespace: String,
  eventHunName: String,
  eventhubParams: Map[String, String],
  progressDir: String) = {
val ssc = new StreamingContext(new SparkContext(), Seconds(batchDuration))
ssc.checkpoint(sparkCheckpointDir)
val inputDirectStream = EventHubsUtils.createDirectStreams(
  ssc,
  namespace,
  progressDir,
  Map(eventHunName -> eventhubParams))

inputDirectStream.map(receivedRecord => (new String(receivedRecord.getBody), 1)).
  reduceByKeyAndWindow((v1, v2) => v1 + v2, (v1, v2) => v1 - v2, Seconds(batchDuration * 3),
    Seconds(batchDuration)).print()

ssc
}

def main(args: Array[String]): Unit = {

if (args.length != 8) {
  println("Usage: program progressDir PolicyName PolicyKey EventHubNamespace EventHubName" +
    " BatchDuration(seconds) Spark_Checkpoint_Directory maxRate")
  sys.exit(1)
}

val progressDir = args(0)
val policyName = args(1)
val policykey = args(2)
val namespace = args(3)
val name = args(4)
val batchDuration = args(5).toInt
val sparkCheckpointDir = args(6)
val maxRate = args(7)

val eventhubParameters = Map[String, String] (
  "eventhubs.policyname" -> policyName,
  "eventhubs.policykey" -> policykey,
  "eventhubs.namespace" -> namespace,
  "eventhubs.name" -> name,
  "eventhubs.partition.count" -> "32",
  "eventhubs.consumergroup" -> "$Default",
  "eventhubs.maxRate" -> s"$maxRate"
)

val ssc = StreamingContext.getOrCreate(sparkCheckpointDir,
  () => createStreamingContext(sparkCheckpointDir, batchDuration, namespace, name,
    eventhubParameters, progressDir))

ssc.start()
ssc.awaitTermination()
}
```

<span data-ttu-id="e5b2d-175">A fenti példában `eventhubParameters` EventHubs egypéldányos vonatkoznak a paramétereket, és végre kell hajtania, hogy a `createDirectStreams` API-hoz létre egy közvetlen DStream objektum leképezésének egy Event Hubs-névtérhez.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-175">In the above example, `eventhubParameters` are the parameters specific to a single EventHubs instance and you have to pass it to the `createDirectStreams` API which constructs a Direct DStream object mapping to a Event Hubs namespace.</span></span> <span data-ttu-id="e5b2d-176">Spark Streamelési API keretrendszer által biztosított DStream API-k hívása az közvetlen DStream objektum.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-176">Over the Direct DStream object, you can call any DStream API provided by Spark Streaming API framework.</span></span> <span data-ttu-id="e5b2d-177">Ebben a példában azt az utolsó 3 micro kötegelt intervallumok belül minden szó gyakoriságát számítható ki.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-177">In this example, we calculate the frequency of each word within the last 3 micro batch intervals.</span></span>

### <a name="receiver-based-connection"></a><span data-ttu-id="e5b2d-178">Fogadó-alapú kapcsolat</span><span class="sxs-lookup"><span data-stu-id="e5b2d-178">Receiver-based Connection</span></span>

<span data-ttu-id="e5b2d-179">A Spark streaming scalában, amely fogadja a eseményeket, és a különböző célok továbbítani, írt mintaalkalmazás érhető el: [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-179">A Spark streaming example application written in Scala, which receives events and route the to different destinations, is available at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span> <span data-ttu-id="e5b2d-180">Az Event Hubs konfigurációját az alkalmazás frissítése és a kimeneti jar létrehozása az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-180">Follow the steps below to update the application for your Event Hub configuration and create the output jar.</span></span>

1. <span data-ttu-id="e5b2d-181">Indítsa el az IntelliJ IDEA, és válassza ki az indítóképernyő **tekintse meg a verziókövetés** majd **Git**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-181">Launch IntelliJ IDEA and from the launch screen select **Check out from Version Control** and then click **Git**.</span></span>
   
    <span data-ttu-id="e5b2d-182">![Apache Spark streaming példa - get forrásokból származó Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming példa - get forrásokból származó Git")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-182">![Apache Spark streaming example - get sources from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming example - get sources from Git")</span></span>

2. <span data-ttu-id="e5b2d-183">Az a **klónozott tárház** párbeszédpanelen adja meg a klónozandó, adja meg a könyvtárat a klón, és kattintson a Git-tárház URL-címe **Klónozás**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-183">In the **Clone Repository** dialog box, provide the URL to the Git repository to clone from, specify the directory to clone to, and then click **Clone**.</span></span>
   
    <span data-ttu-id="e5b2d-184">![Apache Spark streaming példa - klón a Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming példa - klón a Git")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-184">![Apache Spark streaming example - clone from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming example - clone from Git")</span></span>
3. <span data-ttu-id="e5b2d-185">Kövesse az utasításokat, amíg a projekt teljesen klónozták.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-185">Follow the prompts till the project is completely cloned.</span></span> <span data-ttu-id="e5b2d-186">Nyomja le az **Alt + 1** megnyitásához a **Projektnézetében**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-186">Press **Alt + 1** to open the **Project View**.</span></span> <span data-ttu-id="e5b2d-187">A következő kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-187">It should resemble the following.</span></span>
   
    <span data-ttu-id="e5b2d-188">![Apache Spark streaming példa - projekt nézetben](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming példa - projekt nézetben")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-188">![Apache Spark streaming example - Project View](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming example - Project View")</span></span>
4. <span data-ttu-id="e5b2d-189">Győződjön meg arról, hogy az alkalmazás kódjának fordítása során Java8.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-189">Make sure the application code is compiled with Java8.</span></span> <span data-ttu-id="e5b2d-190">Győződjön meg arról, ez, kattintson a **fájl**, kattintson **szerkezetének**, és a a **projekt** lapra, győződjön meg arról, hogy projekt nyelvi szintje **8 - típus jegyzeteket, stb. lambda kifejezések**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-190">To ensure this, click **File**, click **Project Structure**, and on the **Project** tab, make sure Project language level is set to **8 - Lambdas, type annotations, etc.**.</span></span>
   
    <span data-ttu-id="e5b2d-191">![Apache Spark streaming példa - beállítása fordító](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming példa - beállítása fordító")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-191">![Apache Spark streaming example - Set compiler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming example - Set compiler")</span></span>
5. <span data-ttu-id="e5b2d-192">Nyissa meg a **pom.xml** és ellenőrizze, hogy a Spark verziója megfelelő.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-192">Open the **pom.xml** and make sure the Spark version is correct.</span></span> <span data-ttu-id="e5b2d-193">A `<properties>` csomópont, keresse meg a következő kódrészletet, és a Spark-verziójának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-193">Under `<properties>` node, look for the following snippet and verify the Spark version.</span></span>

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. <span data-ttu-id="e5b2d-194">Az alkalmazás futtatásához szükséges egy függőségi jar nevű **JDBC illesztőprogram jar**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-194">The application requires a dependency jar called **JDBC driver jar**.</span></span> <span data-ttu-id="e5b2d-195">Ez szükséges az üzenetek az Event Hubs kapott egy Azure SQL-adatbázisba írni.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-195">This is required to write the messages received from Event Hub into an Azure SQL database.</span></span> <span data-ttu-id="e5b2d-196">Letöltheti a jar (v4.1-es vagy újabb) a [Itt](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-196">You can download this jar (v4.1 or later) from [here](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span></span> <span data-ttu-id="e5b2d-197">Adja hozzá a jar hivatkozást a projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-197">Add reference to this jar in the project library.</span></span> <span data-ttu-id="e5b2d-198">Hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-198">Perform the following steps:</span></span>
     
     1. <span data-ttu-id="e5b2d-199">IntelliJ IDEA ablakban nyissa meg az alkalmazás esetében, kattintson a **fájl**, kattintson a **szerkezetének**, és kattintson a **szalagtárak**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-199">From IntelliJ IDEA window where you have the application open, click **File**, click **Project Structure**, and then click **Libraries**.</span></span> 
     2. <span data-ttu-id="e5b2d-200">Kattintson a Hozzáadás ikonra (![Hozzáadás ikon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), kattintson a **Java**, és navigáljon arra a helyre, amelybe letöltötte a JDBC illesztőprogram jar.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-200">Click the add icon (![add icon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), click **Java**, and then navigate to the location where you downloaded the JDBC driver jar.</span></span> <span data-ttu-id="e5b2d-201">Kövesse az utasításokat a jar-fájlra felvétele a projekt könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-201">Follow the prompts to add the jar file to the project library.</span></span>

         <span data-ttu-id="e5b2d-202">![Adja hozzá a Hiányzó függőségek](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "adja hozzá a hiányzó függőség JAR-fájlok kivételével")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-202">![add missing dependencies](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Add missing dependency jars")</span></span>
     3. <span data-ttu-id="e5b2d-203">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-203">Click **Apply**.</span></span>

7. <span data-ttu-id="e5b2d-204">Hozzon létre a kimeneti jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-204">Create the output jar file.</span></span> <span data-ttu-id="e5b2d-205">A következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-205">Perform the following steps.</span></span>

   1. <span data-ttu-id="e5b2d-206">Az a **szerkezetének** párbeszédpanel, kattintson a **összetevők** és kattintson a plusz jelre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-206">In the **Project Structure** dialog box, click **Artifacts** and then click the plus symbol.</span></span> <span data-ttu-id="e5b2d-207">Az előugró párbeszédpanelen kattintson **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-207">From the pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>      
       
       <span data-ttu-id="e5b2d-208">![Apache Spark streamelési példa - JAR létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamelési példa - JAR létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-208">![Apache Spark streaming example - create JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming example - create JAR")</span></span>
   2. <span data-ttu-id="e5b2d-209">Az a **modulokban létrehozása JAR** párbeszédpanel párbeszédpanelen kattintson a három pont (![három pont](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) szemben a **fő osztály**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-209">In the **Create JAR from Modules** dialog box, click the ellipsis (![ellipsis](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) against the **Main Class**.</span></span>
   3. <span data-ttu-id="e5b2d-210">Az a **fő osztály kiválasztása** párbeszédpanel mezőbe, jelölje be a rendelkezésre álló osztályok bármelyikét, majd kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-210">In the **Select Main Class** dialog box, select any of the available classes and then click **OK**.</span></span>
      
       <span data-ttu-id="e5b2d-211">![Apache Spark streaming példa - jar válassza osztály](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming példa - jar válassza osztály")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-211">![Apache Spark streaming example - select class for jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming example - select class for jar")</span></span>
   4. <span data-ttu-id="e5b2d-212">Az a **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy lehetőség **bontsa ki a cél JAR** van kiválasztva, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-212">In the **Create JAR from Modules** dialog box, make sure that the option to **extract to the target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="e5b2d-213">Ez minden függőség egyetlen JAR hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-213">This creates a single JAR with all dependencies.</span></span>
      
       <span data-ttu-id="e5b2d-214">![Apache Spark streamelési példa - modulok jar létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamelési példa - modulok jar létrehozása")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-214">![Apache Spark streaming example - create jar from modules](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming example - create jar from modules")</span></span>
   5. <span data-ttu-id="e5b2d-215">A **kimeneti elrendezés** lap felsorolja az összes a JAR-fájlok kivételével, amelyek tartalmazzák a Maven project a.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-215">The **Output Layout** tab lists all the jars that are included as part of the Maven project.</span></span> <span data-ttu-id="e5b2d-216">Válassza ki, és törölheti azokat, amelyeken a Scala alkalmazásnak nincs közvetlen függőség.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-216">You can select and delete the ones on which the Scala application has no direct dependency.</span></span> <span data-ttu-id="e5b2d-217">Itt létrehozzuk az alkalmazás is távolítja el a legutóbb (**spark-adatfolyam-adatok-adatmegőrzési-példák fordítási kimeneti**).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-217">For the application we are creating here, you can remove all but the last one (**spark-streaming-data-persistence-examples compile output**).</span></span> <span data-ttu-id="e5b2d-218">A JAR-fájlok kivételével törli, majd válassza ki a **törlése** ikon (![törlés ikonja](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-218">Select the jars to delete and then click the **Delete** icon (![delete icon](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span></span>
      
       <span data-ttu-id="e5b2d-219">![Apache Spark streaming példa - kibontott delete JAR-fájlok kivételével](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming - kibontott delete JAR-fájlok kivételével – példa")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-219">![Apache Spark streaming example - delete extracted jars](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming example - delete extracted jars")</span></span>
      
       <span data-ttu-id="e5b2d-220">Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy a jar minden alkalommal létrejön a projekt beépített vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-220">Make sure **Build on make** box is selected, which ensures that the jar is created every time the project is built or updated.</span></span> <span data-ttu-id="e5b2d-221">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-221">Click **Apply**.</span></span>
   6. <span data-ttu-id="e5b2d-222">Az a **kimeneti elrendezés** lapon jobb alján a **rendelkezésre álló elemek** mezőben van az SQL JDBC jar a projekt könyvtárhoz korábban hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-222">In the **Output Layout** tab, right at the bottom of the **Available Elements** box, you have the SQL JDBC jar that you added earlier to the project library.</span></span> <span data-ttu-id="e5b2d-223">Hozzá kell adnia ezt a **kimeneti elrendezés** fülre. Kattintson a jobb gombbal a jar-fájlra, és kattintson **bontsa ki a kimeneti gyökér**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-223">You must add this to the **Output Layout** tab. Right-click the jar file, and then click **Extract Into Output Root**.</span></span>
      
       <span data-ttu-id="e5b2d-224">![Apache Spark streaming példa - kivonat függőségi jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming példa - kivonat függőségi jar")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-224">![Apache Spark streaming example - extract dependency jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming example - extract dependency jar")</span></span>  
      
       <span data-ttu-id="e5b2d-225">A **kimeneti elrendezés** lapon most példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-225">The **Output Layout** tab should now look like this.</span></span>
      
       <span data-ttu-id="e5b2d-226">![Apache Spark streaming példa - végső kimenetet lapon](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming példa - végső kimenetet lap")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-226">![Apache Spark streaming example - final output tab](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming example - final output tab")</span></span>        
      
       <span data-ttu-id="e5b2d-227">A a **szerkezetének** párbeszédpanel, kattintson a **alkalmaz** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-227">In the **Project Structure** dialog box, click **Apply** and then click **OK**.</span></span>    
   7. <span data-ttu-id="e5b2d-228">A menüsávban kattintson **Build**, és kattintson a **ellenőrizze projekt**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-228">From the menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="e5b2d-229">Is **Build összetevők** a jar létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-229">You can also click **Build Artifacts** to create the jar.</span></span> <span data-ttu-id="e5b2d-230">A kimeneti jar alatt létrejön **\classes\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-230">The output jar is created under **\classes\artifacts**.</span></span>
      
       <span data-ttu-id="e5b2d-231">![Apache Spark streamelési példa - kimeneti JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streamelési példa - kimeneti JAR")</span><span class="sxs-lookup"><span data-stu-id="e5b2d-231">![Apache Spark streaming example - output JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming example - output JAR")</span></span>

## <a name="run-the-application-remotely-on-a-spark-cluster-using-livy"></a><span data-ttu-id="e5b2d-232">Az alkalmazás távoli futtatása Spark-fürtön Livy használatával</span><span class="sxs-lookup"><span data-stu-id="e5b2d-232">Run the application remotely on a Spark cluster using Livy</span></span>

<span data-ttu-id="e5b2d-233">Ebben a cikkben Livy futtatásához használt az Apache Spark streaming alkalmazás távolról Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-233">In this article you use Livy to run the Apache Spark streaming application remotely on a Spark cluster.</span></span> <span data-ttu-id="e5b2d-234">HDInsight Spark-fürtön a Livy használatával részletes leírását lásd: [küldés feladatok távolról Apache Spark on Azure HDInsight fürt](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-234">For detailed discussion on how to use Livy with HDInsight Spark cluster, see [Submit jobs remotely to an Apache Spark cluster on Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span> <span data-ttu-id="e5b2d-235">Mielőtt megkezdené a Spark streaming alkalmazás fut, van néhány dolgot kell tennie:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-235">Before you can start running the Spark streaming application, there are a couple of things you should do:</span></span>

1. <span data-ttu-id="e5b2d-236">Indítsa el a helyi önálló alkalmazás események generálásához, és küldése eseményközpontba.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-236">Start the local standalone application to generate events and sent to Event Hub.</span></span> <span data-ttu-id="e5b2d-237">A következő paranccsal teheti:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-237">Use the following command to do so:</span></span>

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. <span data-ttu-id="e5b2d-238">Másolja át a folyamatos átviteli jar (**spark-adatfolyam-adatok-adatmegőrzési-examples.jar**) a fürthöz tartozó Azure blobtárolóba.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-238">Copy the streaming jar (**spark-streaming-data-persistence-examples.jar**) to the Azure Blob storage associated with the cluster.</span></span> <span data-ttu-id="e5b2d-239">Így a jar Livy érhető el.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-239">This makes the jar accessible to Livy.</span></span> <span data-ttu-id="e5b2d-240">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), parancssori segédprogram, ehhez.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-240">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, to do so.</span></span> <span data-ttu-id="e5b2d-241">Nincsenek sok más ügyfelek segítségével feltölteni az adatokat.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-241">There are a lot of other clients you can use to upload data.</span></span> <span data-ttu-id="e5b2d-242">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-242">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
3. <span data-ttu-id="e5b2d-243">Telepítse a CURL ezeket az alkalmazásokat futtató számítógépre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-243">Install CURL on the computer where you are running these applications from.</span></span> <span data-ttu-id="e5b2d-244">CURL a Livy végpontokat a feladatok távoli futtatása meghívása használjuk.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-244">We use CURL to invoke the Livy endpoints to run the jobs remotely.</span></span>

### <a name="run-the-spark-streaming-application-to-receive-the-events-into-an-azure-storage-blob-as-text"></a><span data-ttu-id="e5b2d-245">A Spark streaming az események fogadásához szövegként egy Azure Storage-Blobból az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-245">Run the Spark streaming application to receive the events into an Azure Storage Blob as text</span></span>

<span data-ttu-id="e5b2d-246">Nyisson meg egy parancssort, keresse meg a CURL telepítési könyvtárát, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt neve):</span><span class="sxs-lookup"><span data-stu-id="e5b2d-246">Open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="e5b2d-247">A paraméterek, a fájl **inputBlob.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-247">The parameters in the file **inputBlob.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="e5b2d-248">Ossza meg velünk megismeréséhez a bemeneti fájl paraméterek:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-248">Let us understand what the parameters in the input file are:</span></span>

* <span data-ttu-id="e5b2d-249">**fájl** elérési útját a fürthöz társított Azure storage-fiók alkalmazás jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-249">**file** is the path to the application jar file on the Azure storage account associated with the cluster.</span></span>
* <span data-ttu-id="e5b2d-250">**Osztálynév** az osztály a jar a neve.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-250">**className** is the name of the class in the jar.</span></span>
* <span data-ttu-id="e5b2d-251">**argumentum** van az osztály által igényelt argumentumok listájának megjelenítése</span><span class="sxs-lookup"><span data-stu-id="e5b2d-251">**args** is the list of arguments required by the class</span></span>
* <span data-ttu-id="e5b2d-252">**numExecutors** adatfolyam futtatása Spark által használt magok száma.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-252">**numExecutors** is the number of cores used by Spark to run the streaming application.</span></span> <span data-ttu-id="e5b2d-253">Mindig legyen legalább kétszerese az Event Hubs partíciók száma.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-253">This should always be at least twice the number of Event Hub partitions.</span></span>
* <span data-ttu-id="e5b2d-254">**executorMemory**, **executorCores**, **driverMemory** szükséges erőforrások hozzárendelése az adatfolyam-továbbítási alkalmazást használt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-254">**executorMemory**, **executorCores**, **driverMemory** are parameters used to assign required resources to the streaming application.</span></span>

> [!NOTE]
> <span data-ttu-id="e5b2d-255">Nem kell létrehozni a kimeneti mappák (EventCheckpoint, EventCount/EventCount10), amelyek paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-255">You do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="e5b2d-256">Az adatfolyam-továbbítási alkalmazás létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-256">The streaming application creates them for you.</span></span>
>
>

<span data-ttu-id="e5b2d-257">A parancs futtatásakor a következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-257">When you run the command, you should see an output like the following:</span></span>

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 to host mysparkcluster.azurehdinsight.net left intact

<span data-ttu-id="e5b2d-258">Jegyezze fel a Kötegazonosító (a példában az értéke "1") a kimenet utolsó sora a.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-258">Make a note of the batch ID in the last line of the output (in this example it is '1').</span></span> <span data-ttu-id="e5b2d-259">Győződjön meg arról, hogy az alkalmazás sikeres futtatása, hogy vessen egy pillantást a a fürthöz tartozó Azure-tárfiókot, és megjelenítheti a **/EventCount/EventCount10** létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-259">To verify that the application runs successfully, you can look at your Azure storage account associated with the cluster and you should see the **/EventCount/EventCount10** folder created there.</span></span> <span data-ttu-id="e5b2d-260">Ebben a mappában kell tartalmaznia, amely rögzíti az idő megadott időtartamon belül a paraméter a feldolgozott események száma blobok **batch-időköz-a-(másodperc)**.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-260">This folder should contain blobs that captures the number of events processed within the time period specified for the parameter **batch-interval-in-seconds**.</span></span>

<span data-ttu-id="e5b2d-261">A Spark streaming alkalmazás továbbra is futnak, amíg nem állítsa le.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-261">The Spark streaming application will continue to run until you kill it.</span></span> <span data-ttu-id="e5b2d-262">Ehhez használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-262">To do so, use the following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-the-applications-to-receive-the-events-into-an-azure-storage-blob-as-json"></a><span data-ttu-id="e5b2d-263">Az alkalmazások az események fogadásához az Azure Storage-Blobba JSON-ként történő futtatása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-263">Run the applications to receive the events into an Azure Storage Blob as JSON</span></span>
<span data-ttu-id="e5b2d-264">Nyisson meg egy parancssort, keresse meg a CURL telepítési könyvtárát, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt neve):</span><span class="sxs-lookup"><span data-stu-id="e5b2d-264">Open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="e5b2d-265">A paraméterek, a fájl **inputJSON.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-265">The parameters in the file **inputJSON.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="e5b2d-266">A paraméterek a szöveges kimenet az előző lépésben megadott hasonlóak.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-266">The parameters are similar to what you specified for the text output, in the previous step.</span></span> <span data-ttu-id="e5b2d-267">Ebben az esetben nem kell létrehozni a kimeneti mappa (EventCheckpoint, EventCount/EventCount10) paraméterekként használt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-267">Again, you do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="e5b2d-268">Az adatfolyam-továbbítási alkalmazás létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-268">The streaming application creates them for you.</span></span>

 <span data-ttu-id="e5b2d-269">Után futtassa a parancsot, megtalálhatja a a fürthöz tartozó Azure-tárfiókot, és megjelenítheti a **/EventStore10** létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-269">After you run the command, you can look at your Azure storage account associated with the cluster and you should see the **/EventStore10** folder created there.</span></span> <span data-ttu-id="e5b2d-270">Nyissa meg a fájl a következő előtaggal **rész -** és a JSON formátumban a feldolgozott események kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-270">Open any file prefixed with **part-** and you should see the events processed in a JSON format.</span></span>

### <a name="run-the-applications-to-receive-the-events-into-a-hive-table"></a><span data-ttu-id="e5b2d-271">A Hive tábla az események fogadásához az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-271">Run the applications to receive the events into a Hive table</span></span>
<span data-ttu-id="e5b2d-272">A Spark streaming alkalmazás, amely egy Hive táblába az események adatfolyamokat futtatásához szükség van néhány további összetevőket.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-272">To run the Spark streaming application that streams events into a Hive table you need some additional components.</span></span> <span data-ttu-id="e5b2d-273">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-273">These are:</span></span>

* <span data-ttu-id="e5b2d-274">datanucleus-api-jdo-3.2.6.jar</span><span class="sxs-lookup"><span data-stu-id="e5b2d-274">datanucleus-api-jdo-3.2.6.jar</span></span>
* <span data-ttu-id="e5b2d-275">datanucleus-rdbms-3.2.9.jar</span><span class="sxs-lookup"><span data-stu-id="e5b2d-275">datanucleus-rdbms-3.2.9.jar</span></span>
* <span data-ttu-id="e5b2d-276">datanucleus-core-3.2.10.jar</span><span class="sxs-lookup"><span data-stu-id="e5b2d-276">datanucleus-core-3.2.10.jar</span></span>
* <span data-ttu-id="e5b2d-277">Hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="e5b2d-277">hive-site.xml</span></span>

<span data-ttu-id="e5b2d-278">A **.jar** fájlok érhetők el: a HDInsight Spark-fürt `/usr/hdp/current/spark-client/lib`.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-278">The **.jar** files are available on your HDInsight Spark cluster at `/usr/hdp/current/spark-client/lib`.</span></span> <span data-ttu-id="e5b2d-279">A **hive-site.xml** érhető el `/usr/hdp/current/spark-client/conf`.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-279">The **hive-site.xml** is available at `/usr/hdp/current/spark-client/conf`.</span></span>

<span data-ttu-id="e5b2d-280">Használhat [WinScp](http://winscp.net/eng/download.php) ezeket a fájlokat másolja a fürtről a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-280">You can use [WinScp](http://winscp.net/eng/download.php) to copy over these files from the cluster to your local computer.</span></span> <span data-ttu-id="e5b2d-281">Eszközök segítségével ezeket a fájlokat másolja át a tárfiókhoz a fürthöz tartozó majd.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-281">You can then use tools to copy these files over to your storage account associated with the cluster.</span></span> <span data-ttu-id="e5b2d-282">Fájlok feltöltése a tárfiókba kapcsolatos további információkért lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-282">For more information on how to upload files to the storage account, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

<span data-ttu-id="e5b2d-283">Miután átmásolta a fájlokat az Azure storage-fiókjába, nyisson meg egy parancssort, keresse meg a CURL telepítési könyvtárát, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt neve):</span><span class="sxs-lookup"><span data-stu-id="e5b2d-283">Once you have copied over the files to your Azure storage account, open a command prompt, navigate to the directory where you installed CURL, and run the following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="e5b2d-284">A paraméterek, a fájl **inputHive.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-284">The parameters in the file **inputHive.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="e5b2d-285">A paraméterek a szöveges kimenet az előző lépésben megadott hasonlóak.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-285">The parameters are similar to what you specified for the text output, in the previous steps.</span></span> <span data-ttu-id="e5b2d-286">Ebben az esetben nem kell létrehozni a kimeneti mappa (EventCheckpoint, EventCount/EventCount10) vagy a kimeneti paraméterként használt Hive tábla (EventHiveTable10).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-286">Again, you do not need to create the output folders (EventCheckpoint, EventCount/EventCount10) or the output Hive table (EventHiveTable10) that are used as parameters.</span></span> <span data-ttu-id="e5b2d-287">Az adatfolyam-továbbítási alkalmazás létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-287">The streaming application creates them for you.</span></span> <span data-ttu-id="e5b2d-288">Vegye figyelembe, hogy a **JAR-fájlok kivételével** és **fájlok** beállítás a .jar fájlt és a hive-site.xml felülírt a tárfiók elérési útjai tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-288">Note that the **jars** and **files** option includes paths to the .jar files and the hive-site.xml that you copied over to the storage account.</span></span>

<span data-ttu-id="e5b2d-289">Győződjön meg arról, hogy a hive tábla létrehozása sikeresen megtörtént, akkor SSH a fürt és a Hive-lekérdezések futtatása.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-289">To verify that the hive table was successfully created, you can SSH into the cluster and run Hive queries.</span></span> <span data-ttu-id="e5b2d-290">Útmutatásért lásd: [használja az SSH a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-290">For instructions, see [Use Hive with Hadoop in HDInsight with SSH](hdinsight-hadoop-use-hive-ssh.md).</span></span> <span data-ttu-id="e5b2d-291">Miután csatlakozott SSH használatával, futtathatja a következő parancs futtatásával ellenőrizze, hogy a Hive tábla **EventHiveTable10**, jön létre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-291">Once you are connected using SSH, you can run the following command to verify that the Hive table, **EventHiveTable10**, is created.</span></span>

    show tables;

<span data-ttu-id="e5b2d-292">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-292">You should see an output similar to the following:</span></span>

    OK
    eventhivetable10
    hivesampletable

<span data-ttu-id="e5b2d-293">A SELECT lekérdezés megtekintéséhez a tábla tartalmát is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-293">You can also run a SELECT query to view the contents of the table.</span></span>

    SELECT * FROM eventhivetable10 LIMIT 10;

<span data-ttu-id="e5b2d-294">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-294">You should see an output like the following:</span></span>

    ZN90apUSQODDTx7n6Toh6jDbuPngqT4c
    sor2M7xsFwmaRW8W8NDwMneFNMrOVkW1
    o2HcsU735ejSi2bGEcbUSB4btCFmI1lW
    TLuibq4rbj0T9st9eEzIWJwNGtMWYoYS
    HKCpPlWFWAJILwR69MAq863nCWYzDEw6
    Mvx0GQOPYvPR7ezBEpIHYKTKiEhYammQ
    85dRppSBSbZgThLr1s0GMgKqynDUqudr
    5LAWkNqorLj3ZN9a2mfWr9rZqeXKN4pF
    ulf9wSFNjD7BZXCyunozecov9QpEIYmJ
    vWzM3nvOja8DhYcwn0n5eTfOItZ966pa
    Time taken: 4.434 seconds, Fetched: 10 row(s)


### <a name="run-the-applications-to-receive-the-events-into-an-azure-sql-database-table"></a><span data-ttu-id="e5b2d-295">Az események fogadásához az Azure SQL adatbázis táblába az alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-295">Run the applications to receive the events into an Azure SQL database table</span></span>
<span data-ttu-id="e5b2d-296">Ez a lépés futtatása előtt győződjön meg arról, hogy az Azure SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-296">Before running this step, make sure you have an Azure SQL database created.</span></span> <span data-ttu-id="e5b2d-297">Útmutatásért lásd: [SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-297">For instructions, see [Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span> <span data-ttu-id="e5b2d-298">Ez a szakasz befejezéséhez kell értékek adatbázisnév, adatbázis-kiszolgáló nevét és az adatbázis rendszergazdájának hitelesítő adatait a paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-298">To complete this section, you need values for database name, database server name, and the database administrator credentials as parameters.</span></span> <span data-ttu-id="e5b2d-299">Nem kell a adatbázistábla létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-299">You do not need to create the database table though.</span></span> <span data-ttu-id="e5b2d-300">A Spark streaming alkalmazást hoz létre, amely meg.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-300">The Spark streaming application creates that for you.</span></span>

<span data-ttu-id="e5b2d-301">Nyisson meg egy parancssort, keresse meg a CURL telepítési könyvtárát, és futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-301">Open a command prompt, navigate to the directory where you installed CURL, and run the following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="e5b2d-302">A paraméterek, a fájl **inputSQL.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-302">The parameters in the file **inputSQL.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="e5b2d-303">Győződjön meg arról, hogy az alkalmazás sikeres futtatása, akkor is képes csatlakozni az Azure SQL-adatbázis SQL Server Management Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-303">To verify that the application runs successfully, you can connect to the Azure SQL database using SQL Server Management Studio.</span></span> <span data-ttu-id="e5b2d-304">Ehhez útmutatást lásd: [Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="e5b2d-304">For instructions on how to do that, see [Connect to SQL Database with SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span></span> <span data-ttu-id="e5b2d-305">Miután csatlakozott az adatbázishoz, lépjen a **EventContent** táblázatot, amely az adatfolyam-továbbítási alkalmazást hozta létre.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-305">Once you are connected to the database, you can navigate to the **EventContent** table that was created by the streaming application.</span></span> <span data-ttu-id="e5b2d-306">Az adatok lekérése a táblából gyors lekérdezés futtatása.</span><span class="sxs-lookup"><span data-stu-id="e5b2d-306">You can run a quick query to get the data from the table.</span></span> <span data-ttu-id="e5b2d-307">Futtassa a következő lekérdezést:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-307">Run the following query:</span></span>

    SELECT * FROM EventCount

<span data-ttu-id="e5b2d-308">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-308">You should see output similar to the following:</span></span>

    00046b0f-2552-4980-9c3f-8bba5647c8ee
    000b7530-12f9-4081-8e19-90acd26f9c0c
    000bc521-9c1b-4a42-ab08-dc1893b83f3b
    00123a2a-e00d-496a-9104-108920955718
    0017c68f-7a4e-452d-97ad-5cb1fe5ba81b
    001KsmqL2gfu5ZcuQuTqTxQvVyGCqPp9
    001vIZgOStka4DXtud0e3tX7XbfMnZrN
    00220586-3e1a-4d2d-a89b-05c5892e541a
    0029e309-9e54-4e1b-84be-cd04e6fce5ec
    003333cf-874f-4045-9da3-9f98c2b4ea49
    0043c07e-8d73-420a-9af7-1fcb94575356
    004a11a9-0c2c-4bc0-a7d5-2e0ebd947ab9


## <span data-ttu-id="e5b2d-309"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="e5b2d-309"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="e5b2d-310">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="e5b2d-310">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)
* [<span data-ttu-id="e5b2d-311">Fogadó-alapú kapcsolat és a közvetlen DStream tervezése</span><span class="sxs-lookup"><span data-stu-id="e5b2d-311">Design of Receiver-based Connection and Direct DStream</span></span>](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a><span data-ttu-id="e5b2d-312">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="e5b2d-312">Scenarios</span></span>
* [<span data-ttu-id="e5b2d-313">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="e5b2d-313">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="e5b2d-314">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="e5b2d-314">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="e5b2d-315">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="e5b2d-315">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="e5b2d-316">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="e5b2d-316">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="e5b2d-317">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="e5b2d-317">Create and run applications</span></span>
* [<span data-ttu-id="e5b2d-318">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="e5b2d-318">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="e5b2d-319">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="e5b2d-319">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="e5b2d-320">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="e5b2d-320">Tools and extensions</span></span>
* [<span data-ttu-id="e5b2d-321">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark Scala-alkalmazások létrehozásához és elküldéséhez</span><span class="sxs-lookup"><span data-stu-id="e5b2d-321">Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="e5b2d-322">Az IntelliJ IDEA HDInsight-eszközei beépülő moduljának használata Spark-alkalmazások távoli hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="e5b2d-322">Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="e5b2d-323">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="e5b2d-323">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="e5b2d-324">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="e5b2d-324">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="e5b2d-325">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="e5b2d-325">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="e5b2d-326">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="e5b2d-326">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="e5b2d-327">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="e5b2d-327">Manage resources</span></span>
* [<span data-ttu-id="e5b2d-328">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e5b2d-328">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="e5b2d-329">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="e5b2d-329">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
