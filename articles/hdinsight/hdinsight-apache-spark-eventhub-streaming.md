---
title: aaaUse Apache Spark streaming az Event Hubs az Azure HDInsight |} Microsoft Docs
description: "Az Apache Spark streamelési minta build toosend egy adatok tooAzure Eseményközpont-adatfolyam és majd scala-alkalmazások segítségével HDInsight Spark-fürt kap, ezeket az eseményeket."
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
ms.openlocfilehash: 10cc5884047b3b8249fe8a8822a16a19780a4af3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="apache-spark-streaming-process-data-from-azure-event-hubs-with-spark-cluster-on-hdinsight"></a><span data-ttu-id="653bd-104">Apache Spark streaming: a HDInsight fürt adatfeldolgozásra Spark az Azure Event hubs</span><span class="sxs-lookup"><span data-stu-id="653bd-104">Apache Spark streaming: Process data from Azure Event Hubs with Spark cluster on HDInsight</span></span>

<span data-ttu-id="653bd-105">Ebben a cikkben az Apache Spark streaming mintát, amely magában foglalja az alábbi lépésekkel hello hoz létre:</span><span class="sxs-lookup"><span data-stu-id="653bd-105">In this article, you create an Apache Spark streaming sample that involves hello following steps:</span></span>

1. <span data-ttu-id="653bd-106">Az Azure Event Hubs egy önálló alkalmazás tooingest üzenetek használhatja.</span><span class="sxs-lookup"><span data-stu-id="653bd-106">You use a standalone application tooingest messages into an Azure Event Hub.</span></span>

2. <span data-ttu-id="653bd-107">A két különböző szempontok, akkor hello üzeneteket beolvasni az Event Hubs a valós idejű Azure HDInsight Spark-fürtön futó alkalmazást használ.</span><span class="sxs-lookup"><span data-stu-id="653bd-107">With two different approaches, you retrieve hello messages from Event Hub in real-time using an application running in Spark cluster on Azure HDInsight.</span></span>

3. <span data-ttu-id="653bd-108">Adatfolyam-továbbítási elemzőfolyamatokat toopersist adatok toodifferent tárolási rendszereket építhetnek, vagy mélyebb betekintés az adatok hello menet közben.</span><span class="sxs-lookup"><span data-stu-id="653bd-108">You build streaming analytic pipelines toopersist data toodifferent storage systems, or get insights from data on hello fly.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="653bd-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="653bd-109">Prerequisites</span></span>

* <span data-ttu-id="653bd-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="653bd-110">An Azure subscription.</span></span> <span data-ttu-id="653bd-111">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="653bd-111">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>

* <span data-ttu-id="653bd-112">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="653bd-112">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="653bd-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>

## <a name="spark-streaming-concepts"></a><span data-ttu-id="653bd-114">Spark Streaming fogalmak</span><span class="sxs-lookup"><span data-stu-id="653bd-114">Spark Streaming concepts</span></span>

<span data-ttu-id="653bd-115">A Spark streamelési részletes ismertetése, [Apache Spark streaming áttekintése](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span><span class="sxs-lookup"><span data-stu-id="653bd-115">For a detailed explanation of Spark streaming, see [Apache Spark streaming overview](http://spark.apache.org/docs/latest/streaming-programming-guide.html#overview).</span></span> <span data-ttu-id="653bd-116">HDInsight során az Azure-fürt azonos adatfolyam-továbbítási funkciók tooa Spark hello.</span><span class="sxs-lookup"><span data-stu-id="653bd-116">HDInsight brings hello same streaming features tooa Spark cluster on Azure.</span></span>  

## <a name="what-does-this-solution-do"></a><span data-ttu-id="653bd-117">Mire ehhez a megoldáshoz?</span><span class="sxs-lookup"><span data-stu-id="653bd-117">What does this solution do?</span></span>

<span data-ttu-id="653bd-118">Ebben a cikkben, Spark streamelési példa toocreate hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-118">In this article, toocreate a Spark streaming example, perform hello following steps:</span></span>

1. <span data-ttu-id="653bd-119">Hozzon létre egy Azure Event hubs Eseményközpontot, akik szintén megkapják az események adatfolyam.</span><span class="sxs-lookup"><span data-stu-id="653bd-119">Create an Azure Event Hub that will receive a stream of events.</span></span>

2. <span data-ttu-id="653bd-120">Futtassa egy helyi önálló alkalmazás, amely eseményeket hoz létre, és leküldéses értesítések az Azure Event Hubs toohello.</span><span class="sxs-lookup"><span data-stu-id="653bd-120">Run a local standalone application that generates events and pushes it toohello Azure Event Hub.</span></span> <span data-ttu-id="653bd-121">hello mintaalkalmazás, amely ezt a közzétett [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="653bd-121">hello sample application that does this is published at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span>

3. <span data-ttu-id="653bd-122">Távolról futtatni egy adatfolyam-továbbítási alkalmazást egy Spark-fürt, amely az Azure Event Hubs olvassa be az esemény streamelését és végezhetnek különböző adatfeldolgozási /.</span><span class="sxs-lookup"><span data-stu-id="653bd-122">Run a streaming application remotely on a Spark cluster that reads streaming events from Azure Event Hub and perform various data processing/analysis.</span></span>

## <a name="create-an-azure-event-hub"></a><span data-ttu-id="653bd-123">Hozzon létre egy Azure Event Hubs</span><span class="sxs-lookup"><span data-stu-id="653bd-123">Create an Azure Event Hub</span></span>

1. <span data-ttu-id="653bd-124">Jelentkezzen be toohello [Azure Portal](https://ms.portal.azure.com), és kattintson **új** hello: bal felső részén üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="653bd-124">Log on toohello [Azure Portal](https://ms.portal.azure.com), and click **New** at hello top left of hello screen.</span></span>

2. <span data-ttu-id="653bd-125">Kattintson az **Eszközök internetes hálózata**, majd az **Event Hubs** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="653bd-125">Click **Internet of Things**, then click **Event Hubs**.</span></span>

    <span data-ttu-id="653bd-126">![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Spark streamelési példa eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="653bd-126">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-create-event-hub-for-spark-streaming.png "Create event hub for Spark streaming example")</span></span>

3. <span data-ttu-id="653bd-127">A hello **névtér létrehozása** panelen adja meg a névtér nevét.</span><span class="sxs-lookup"><span data-stu-id="653bd-127">In hello **Create namespace** blade, enter a namespace name.</span></span> <span data-ttu-id="653bd-128">Válassza ki a hello tarifacsomag (alapszintű vagy Standard).</span><span class="sxs-lookup"><span data-stu-id="653bd-128">choose hello pricing tier (Basic or Standard).</span></span> <span data-ttu-id="653bd-129">Válassza ki, egy Azure-előfizetéssel, erőforráscsoportot és helyet az erőforrás-toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="653bd-129">Also, choose an Azure subscription, resource group, and location in which toocreate hello resource.</span></span> <span data-ttu-id="653bd-130">Kattintson a **létrehozása** toocreate hello névtér.</span><span class="sxs-lookup"><span data-stu-id="653bd-130">Click **Create** toocreate hello namespace.</span></span>

      <span data-ttu-id="653bd-131">![Adjon meg a Spark streamelési példa event hub nevet](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "adja meg az eseményközpont neveként a Spark streamelési – példa")</span><span class="sxs-lookup"><span data-stu-id="653bd-131">![Provide an event hub name for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-name-for-spark-streaming.png "Provide an event hub name for Spark streaming example")</span></span>

    > [!NOTE]
    > <span data-ttu-id="653bd-132">Meg kell válasszon hello azonos **hely** a Apache Spark-fürt a HDInsight tooreduce késleltetés és a költségek.</span><span class="sxs-lookup"><span data-stu-id="653bd-132">You should select hello same **Location** as your Apache Spark cluster in HDInsight tooreduce latency and costs.</span></span>
    >
    >

4. <span data-ttu-id="653bd-133">Hello Event Hubs-névtér listájában kattintson hello újonnan létrehozott névtér.</span><span class="sxs-lookup"><span data-stu-id="653bd-133">In hello Event Hubs namespace list, click hello newly-created namespace.</span></span>      


5. <span data-ttu-id="653bd-134">Hello névtér paneljén kattintson **Event Hubs**, és kattintson a **+ Eseményközpont** toocreate egy új Eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="653bd-134">In hello namespace blade, click **Event Hubs**, and then click **+ Event Hub** toocreate a new Event Hub.</span></span>
   
    <span data-ttu-id="653bd-135">![Spark streaming példa eseményközpont létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Spark streamelési példa eseményközpont létrehozása")</span><span class="sxs-lookup"><span data-stu-id="653bd-135">![Create event hub for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-open-event-hubs-blade-for-spark-streaming-example.png "Create event hub for Spark streaming example")</span></span>

6. <span data-ttu-id="653bd-136">Adjon meg egy nevet az Eseményközpont set hello partíció száma too10 és üzenet megőrzési too1.</span><span class="sxs-lookup"><span data-stu-id="653bd-136">Type a name for your Event Hub, set hello partition count too10, and message retention too1.</span></span> <span data-ttu-id="653bd-137">Jelenleg nem archiválni köszönőüzenetei ebben a megoldásban, az alapértelmezett hello rest hagyja, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="653bd-137">We are not archiving hello messages in this solution so you can leave hello rest as default, and then click **Create**.</span></span>
   
    <span data-ttu-id="653bd-138">![Adja meg a Spark streaming példa event hub-adatok](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Spark streamelési példa event hub-adatok megadása")</span><span class="sxs-lookup"><span data-stu-id="653bd-138">![Provide event hub details for Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-provide-event-hub-details-for-spark-streaming-example.png "Provide event hub details for Spark streaming example")</span></span>

7. <span data-ttu-id="653bd-139">az újonnan létrehozott Eseményközpont hello hello Event Hub panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="653bd-139">hello newly created Event Hub is listed in hello Event Hub blade.</span></span>
    
     <span data-ttu-id="653bd-140">![Az Event Hubs megtekintése hello Spark streamelési például](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "nézet Eseményközpont hello Spark streamelési – példa")</span><span class="sxs-lookup"><span data-stu-id="653bd-140">![View Event Hub for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-for-spark-streaming-example.png "View Event Hub for hello Spark streaming example")</span></span>

8. <span data-ttu-id="653bd-141">Hello névtér (nem hello adott Event Hub panel) panelen, kattintson **megosztott elérési házirendek**, és kattintson a **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="653bd-141">Back in hello namespace blade (not hello specific Event Hub blade), click **Shared access policies**, and then click **RootManageSharedAccessKey**.</span></span>
    
     <span data-ttu-id="653bd-142">![Az Event Hubs házirendbe hello Spark streamelési példa](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "hello szabályzatok beállítása az Event Hubs Spark streamelési – példa")</span><span class="sxs-lookup"><span data-stu-id="653bd-142">![Set Event Hub policies for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-set-event-hub-policies-for-spark-streaming-example.png "Set Event Hub policies for hello Spark streaming example")</span></span>

9. <span data-ttu-id="653bd-143">Kattintson a hello Másolás gombra toocopy hello **RootManageSharedAccessKey** elsődleges kulcs és a kapcsolati karakterlánc toohello vágólapra.</span><span class="sxs-lookup"><span data-stu-id="653bd-143">Click hello copy button toocopy hello **RootManageSharedAccessKey** primary key and connection string toohello clipboard.</span></span> <span data-ttu-id="653bd-144">Mentse a toouse hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="653bd-144">Save these toouse later in hello tutorial.</span></span>
    
     <span data-ttu-id="653bd-145">![Az Event Hubs házirend kulcsok hello Spark streamelési például megtekintése](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "nézet Eseményközpont házirend kulcsokat hello Spark streamelési – példa")</span><span class="sxs-lookup"><span data-stu-id="653bd-145">![View Event Hub policy keys for hello Spark streaming example](./media/hdinsight-apache-spark-eventhub-streaming/hdinsight-view-event-hub-policy-keys.png "View Event Hub policy keys for hello Spark streaming example")</span></span>

## <a name="send-messages-tooazure-event-hub-using-a-sample-scala-application"></a><span data-ttu-id="653bd-146">Az Event Hubs egy mintaalkalmazás Scala használatával üzenetek tooAzure küldése</span><span class="sxs-lookup"><span data-stu-id="653bd-146">Send messages tooAzure Event Hub using a sample Scala application</span></span>

<span data-ttu-id="653bd-147">Ebben a szakaszban egy különálló helyi Scala alkalmazás, amely események adatfolyam hoz létre, és elküldi azt korábban létrehozott Eseményközpont tooAzure használja.</span><span class="sxs-lookup"><span data-stu-id="653bd-147">In this section you use a standalone local Scala application that generates a stream of events and sends it tooAzure Event Hub that you created earlier.</span></span> <span data-ttu-id="653bd-148">Ez az alkalmazás érhető el a Githubon: [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span><span class="sxs-lookup"><span data-stu-id="653bd-148">This application is available on GitHub at [https://github.com/hdinsight/eventhubs-sample-event-producer](https://github.com/hdinsight/eventhubs-sample-event-producer).</span></span> <span data-ttu-id="653bd-149">Itt a hello lépések azt feltételezik, hogy Ön rendelkezik már ágazik el a GitHub-tárházban.</span><span class="sxs-lookup"><span data-stu-id="653bd-149">hello steps here assume that you have already forked this GitHub repository.</span></span>

1. <span data-ttu-id="653bd-150">Ellenőrizze, hogy a hello következő hello ezt az alkalmazást futtató számítógépre telepített.</span><span class="sxs-lookup"><span data-stu-id="653bd-150">Make sure you have hello following installed on hello computer where you run this application.</span></span>

    * <span data-ttu-id="653bd-151">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="653bd-151">Oracle Java Development kit.</span></span> <span data-ttu-id="653bd-152">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="653bd-152">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
    * <span data-ttu-id="653bd-153">Apache Maven.</span><span class="sxs-lookup"><span data-stu-id="653bd-153">Apache Maven.</span></span> <span data-ttu-id="653bd-154">Letöltheti a [Itt](https://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="653bd-154">You can download it from [here](https://maven.apache.org/download.cgi).</span></span> <span data-ttu-id="653bd-155">Útmutatás tooinstall Maven érhetők el [Itt](https://maven.apache.org/install.html).</span><span class="sxs-lookup"><span data-stu-id="653bd-155">Instructions tooinstall Maven are available [here](https://maven.apache.org/install.html).</span></span>

2. <span data-ttu-id="653bd-156">Nyisson meg egy parancssort, és keresse meg a klónozott hello GitHub-tárház hello Scala mintaalkalmazás toohello helyét, és futtassa a következő parancs toobuild hello alkalmazás hello.</span><span class="sxs-lookup"><span data-stu-id="653bd-156">Open a command prompt and navigate toohello location you cloned hello GitHub repo for hello sample Scala application and run hello following command toobuild hello application.</span></span>

        mvn package

3. <span data-ttu-id="653bd-157">hello kimeneti jar hello alkalmazás **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, alatt létrejön **/target** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="653bd-157">hello output jar for hello application, **com-microsoft-azure-eventhubs-client-example-0.2.0.jar**, is created under **/target** directory.</span></span> <span data-ttu-id="653bd-158">Ez a cikk tootest hello teljes megoldás később JAR használhatja.</span><span class="sxs-lookup"><span data-stu-id="653bd-158">You use this JAR later in this article tootest hello complete solution.</span></span>

## <a name="create-application-tooreceive-messages-from-event-hub-into-a-spark-cluster"></a><span data-ttu-id="653bd-159">Az Event Hubs tooreceive alkalmazásüzeneteket a Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="653bd-159">Create application tooreceive messages from Event Hub into a Spark cluster</span></span> 

<span data-ttu-id="653bd-160">Spark Streaming két megközelítés tooconnect és az Azure Event Hubs, a fogadó-alapú kapcsolat és a közvetlen DStream-alapú kapcsolat van.</span><span class="sxs-lookup"><span data-stu-id="653bd-160">We have two approaches tooconnect Spark Streaming and Azure Event Hubs, Receiver-based connection and Direct-DStream-based connection.</span></span> <span data-ttu-id="653bd-161">Közvetlen DStream-alapú van a Jan 2017 hello 2.0.3 kiadásban bevezetett.</span><span class="sxs-lookup"><span data-stu-id="653bd-161">Direct-DStream-based is introduced on Jan of 2017, in hello 2.0.3 release.</span></span> <span data-ttu-id="653bd-162">Azt kellene tooreplace hello eredeti fogadó-alapú kapcsolat, mert az több performant és erőforrás-hatékony.</span><span class="sxs-lookup"><span data-stu-id="653bd-162">It is supposed tooreplace hello original receiver-based connection as it is more performant and resource-efficient.</span></span> <span data-ttu-id="653bd-163">További részletek találhatók [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span><span class="sxs-lookup"><span data-stu-id="653bd-163">More details found in [https://github.com/hdinsight/spark-eventhubs](https://github.com/hdinsight/spark-eventhubs).</span></span> <span data-ttu-id="653bd-164">Közvetlen DStream csak a Spark 2.0 + támogatja.</span><span class="sxs-lookup"><span data-stu-id="653bd-164">Direct DStream only supports Spark 2.0+.</span></span>

### <a name="build-applications-with-hello-dependency-toospark-eventhubs-connector"></a><span data-ttu-id="653bd-165">Hello függőségi toospark-eventhubs összekötőn keresztül használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="653bd-165">Build applications with hello dependency toospark-eventhubs connector</span></span>

<span data-ttu-id="653bd-166">Azt is közzétesz hello átmeneti Spark-EventHubs a Githubon verzióját.</span><span class="sxs-lookup"><span data-stu-id="653bd-166">We will also publish hello staging version of Spark-EventHubs in GitHub.</span></span> <span data-ttu-id="653bd-167">toouse hello átmeneti a Spark-EventHubs, hello első lépéseként verziója tooindicate GitHub, a forrás-tárház hello adja hozzá a következő bejegyzés toopom.xml hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-167">toouse hello staging version of Spark-EventHubs, hello first step is tooindicate GitHub as hello source repo by adding hello following entry toopom.xml:</span></span>

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

<span data-ttu-id="653bd-168">A következő függőségi tooyour projekt tootake hello előzetes kiadását hello majd adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="653bd-168">You can then add hello following dependency tooyour project tootake hello pre-released version.</span></span>

<span data-ttu-id="653bd-169">Maven-függőség</span><span class="sxs-lookup"><span data-stu-id="653bd-169">Maven Dependency</span></span>

```xml
<!-- https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11 -->
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>spark-streaming-eventhubs_2.11</artifactId>
    <version>2.0.4</version>
</dependency>
```

<span data-ttu-id="653bd-170">SBT függőség</span><span class="sxs-lookup"><span data-stu-id="653bd-170">SBT Dependency</span></span>

```
// https://mvnrepository.com/artifact/com.microsoft.azure/spark-streaming-eventhubs_2.11
libraryDependencies += "com.microsoft.azure" % "spark-streaming-eventhubs_2.11" % "2.0.4"
```

### <a name="direct-dstream-connection"></a><span data-ttu-id="653bd-171">Közvetlen DStream kapcsolat</span><span class="sxs-lookup"><span data-stu-id="653bd-171">Direct DStream Connection</span></span>

<span data-ttu-id="653bd-172">A letölthető egy előre elkészített jar-fájlt tartalmazó példák segítségével közvetlen DStream [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span><span class="sxs-lookup"><span data-stu-id="653bd-172">A pre-built jar file containing examples using Direct DStream can be downloaded in [http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar](http://central.maven.org/maven2/com/microsoft/azure/spark-streaming-eventhubs_2.11/2.0.4/spark-streaming-eventhubs_2.11-2.0.4.jar).</span></span>

<span data-ttu-id="653bd-173">hello jar-fájlt tartalmaz három példák, amelyek a forráskód érhetők el [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span><span class="sxs-lookup"><span data-stu-id="653bd-173">hello jar file contains three examples whose source code are available at [https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream](https://github.com/hdinsight/spark-eventhubs/tree/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream).</span></span>

<span data-ttu-id="653bd-174">Véve [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) példa:</span><span class="sxs-lookup"><span data-stu-id="653bd-174">Taking [WindowingWordCount](https://github.com/hdinsight/spark-eventhubs/blob/master/examples/src/main/scala/com/microsoft/spark/streaming/examples/directdstream/WindowingWordCount.scala) as an example:</span></span>

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

<span data-ttu-id="653bd-175">A fenti példában hello `eventhubParameters` vannak hello paraméterek adott tooa EventHubs egypéldányos, és rendelkezik toopass azt toohello `createDirectStreams` API-hoz létre egy közvetlen DStream objektum leképezési tooa Event Hubs névtér.</span><span class="sxs-lookup"><span data-stu-id="653bd-175">In hello above example, `eventhubParameters` are hello parameters specific tooa single EventHubs instance and you have toopass it toohello `createDirectStreams` API which constructs a Direct DStream object mapping tooa Event Hubs namespace.</span></span> <span data-ttu-id="653bd-176">Spark Streamelési API keretrendszer által biztosított DStream API-k hívása hello közvetlen DStream objektum.</span><span class="sxs-lookup"><span data-stu-id="653bd-176">Over hello Direct DStream object, you can call any DStream API provided by Spark Streaming API framework.</span></span> <span data-ttu-id="653bd-177">Ebben a példában a Microsoft hello utolsó 3 micro kötegelt időközök belül minden szó hello gyakorisága számítható ki.</span><span class="sxs-lookup"><span data-stu-id="653bd-177">In this example, we calculate hello frequency of each word within hello last 3 micro batch intervals.</span></span>

### <a name="receiver-based-connection"></a><span data-ttu-id="653bd-178">Fogadó-alapú kapcsolat</span><span class="sxs-lookup"><span data-stu-id="653bd-178">Receiver-based Connection</span></span>

<span data-ttu-id="653bd-179">A Spark streaming események és útvonal hello toodifferent célok kap, scalában írt mintaalkalmazás érhető el: [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span><span class="sxs-lookup"><span data-stu-id="653bd-179">A Spark streaming example application written in Scala, which receives events and route hello toodifferent destinations, is available at [https://github.com/hdinsight/spark-streaming-data-persistence-examples](https://github.com/hdinsight/spark-streaming-data-persistence-examples).</span></span> <span data-ttu-id="653bd-180">Kövesse az alábbi tooupdate hello alkalmazásban az Eseményközpont konfigurációja hello lépéseket, és hozzon létre hello kimeneti jar.</span><span class="sxs-lookup"><span data-stu-id="653bd-180">Follow hello steps below tooupdate hello application for your Event Hub configuration and create hello output jar.</span></span>

1. <span data-ttu-id="653bd-181">Indítsa el az IntelliJ IDEA, és a hello indítsa el a képernyőn válassza ki **tekintse meg a verziókövetés** majd **Git**.</span><span class="sxs-lookup"><span data-stu-id="653bd-181">Launch IntelliJ IDEA and from hello launch screen select **Check out from Version Control** and then click **Git**.</span></span>
   
    <span data-ttu-id="653bd-182">![Apache Spark streaming példa - get forrásokból származó Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming példa - get forrásokból származó Git")</span><span class="sxs-lookup"><span data-stu-id="653bd-182">![Apache Spark streaming example - get sources from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-get-source-from-git.png "Apache Spark streaming example - get sources from Git")</span></span>

2. <span data-ttu-id="653bd-183">A hello **klónozott tárház** párbeszédpanelen adja meg a hello URL-cím toohello Git tárház tooclone származó, adja meg a hello directory tooclone, és kattintson **Klónozás**.</span><span class="sxs-lookup"><span data-stu-id="653bd-183">In hello **Clone Repository** dialog box, provide hello URL toohello Git repository tooclone from, specify hello directory tooclone to, and then click **Clone**.</span></span>
   
    <span data-ttu-id="653bd-184">![Apache Spark streaming példa - klón a Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming példa - klón a Git")</span><span class="sxs-lookup"><span data-stu-id="653bd-184">![Apache Spark streaming example - clone from Git](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-clone-from-git.png "Apache Spark streaming example - clone from Git")</span></span>
3. <span data-ttu-id="653bd-185">Hello útmutatást követve keretein belül hello projekt teljesen klónozták.</span><span class="sxs-lookup"><span data-stu-id="653bd-185">Follow hello prompts till hello project is completely cloned.</span></span> <span data-ttu-id="653bd-186">Nyomja le az **Alt + 1** tooopen hello **Projektnézetében**.</span><span class="sxs-lookup"><span data-stu-id="653bd-186">Press **Alt + 1** tooopen hello **Project View**.</span></span> <span data-ttu-id="653bd-187">Az alábbi hello kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="653bd-187">It should resemble hello following.</span></span>
   
    <span data-ttu-id="653bd-188">![Apache Spark streaming példa - projekt nézetben](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming példa - projekt nézetben")</span><span class="sxs-lookup"><span data-stu-id="653bd-188">![Apache Spark streaming example - Project View](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-project-view.png "Apache Spark streaming example - Project View")</span></span>
4. <span data-ttu-id="653bd-189">Ellenőrizze, hogy hello alkalmazáskód fordítása során Java8.</span><span class="sxs-lookup"><span data-stu-id="653bd-189">Make sure hello application code is compiled with Java8.</span></span> <span data-ttu-id="653bd-190">tooensure, kattintson **fájl**, kattintson a **szerkezetének**, és a hello **projekt** lapra, győződjön meg arról, hogy a projekt nyelvi szintje túl**8 - lambda kifejezések, típusa jegyzetek stb**.</span><span class="sxs-lookup"><span data-stu-id="653bd-190">tooensure this, click **File**, click **Project Structure**, and on hello **Project** tab, make sure Project language level is set too**8 - Lambdas, type annotations, etc.**.</span></span>
   
    <span data-ttu-id="653bd-191">![Apache Spark streaming példa - beállítása fordító](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming példa - beállítása fordító")</span><span class="sxs-lookup"><span data-stu-id="653bd-191">![Apache Spark streaming example - Set compiler](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-java-8-compiler.png "Apache Spark streaming example - Set compiler")</span></span>
5. <span data-ttu-id="653bd-192">Nyissa meg hello **pom.xml** és ellenőrizze, hogy hello Spark verziója megfelelő.</span><span class="sxs-lookup"><span data-stu-id="653bd-192">Open hello **pom.xml** and make sure hello Spark version is correct.</span></span> <span data-ttu-id="653bd-193">A `<properties>` csomópont, keresse meg a következő kódrészletet hello és hello Spark verziójának ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="653bd-193">Under `<properties>` node, look for hello following snippet and verify hello Spark version.</span></span>

        <scala.version>2.11.8</scala.version>
        <scala.compat.version>2.11.8</scala.compat.version>
        <scala.binary.version>2.11</scala.binary.version>
        <spark.version>2.0.0</spark.version>

6. <span data-ttu-id="653bd-194">hello az alkalmazáshoz egy függőségi jar nevű **JDBC illesztőprogram jar**.</span><span class="sxs-lookup"><span data-stu-id="653bd-194">hello application requires a dependency jar called **JDBC driver jar**.</span></span> <span data-ttu-id="653bd-195">Ez az Event Hubs kapott az Azure SQL adatbázishoz szükséges toowrite köszönőüzenetei.</span><span class="sxs-lookup"><span data-stu-id="653bd-195">This is required toowrite hello messages received from Event Hub into an Azure SQL database.</span></span> <span data-ttu-id="653bd-196">Letöltheti a jar (v4.1-es vagy újabb) a [Itt](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span><span class="sxs-lookup"><span data-stu-id="653bd-196">You can download this jar (v4.1 or later) from [here](https://msdn.microsoft.com/sqlserver/aa937724.aspx).</span></span> <span data-ttu-id="653bd-197">Adja hozzá a hivatkozás toothis jar hello projekt könyvtárában.</span><span class="sxs-lookup"><span data-stu-id="653bd-197">Add reference toothis jar in hello project library.</span></span> <span data-ttu-id="653bd-198">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-198">Perform hello following steps:</span></span>
     
     1. <span data-ttu-id="653bd-199">IntelliJ IDEA ablakban nyissa meg a hello alkalmazás esetében, kattintson a **fájl**, kattintson a **szerkezetének**, és kattintson a **szalagtárak**.</span><span class="sxs-lookup"><span data-stu-id="653bd-199">From IntelliJ IDEA window where you have hello application open, click **File**, click **Project Structure**, and then click **Libraries**.</span></span> 
     2. <span data-ttu-id="653bd-200">Hello kattintson hozzáadása ikon (![hozzáadása ikon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), kattintson a **Java**, és navigáljon a hello JDBC illesztőprogram jar kezelőportálon toohello helyét.</span><span class="sxs-lookup"><span data-stu-id="653bd-200">Click hello add icon (![add icon](./media/hdinsight-apache-spark-eventhub-streaming/add-icon.png)), click **Java**, and then navigate toohello location where you downloaded hello JDBC driver jar.</span></span> <span data-ttu-id="653bd-201">Hajtsa végre a hello kér tooadd hello jar fájl toohello projekt könyvtár.</span><span class="sxs-lookup"><span data-stu-id="653bd-201">Follow hello prompts tooadd hello jar file toohello project library.</span></span>

         <span data-ttu-id="653bd-202">![Adja hozzá a Hiányzó függőségek](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "adja hozzá a hiányzó függőség JAR-fájlok kivételével")</span><span class="sxs-lookup"><span data-stu-id="653bd-202">![add missing dependencies](./media/hdinsight-apache-spark-eventhub-streaming/add-missing-dependency-jars.png "Add missing dependency jars")</span></span>
     3. <span data-ttu-id="653bd-203">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="653bd-203">Click **Apply**.</span></span>

7. <span data-ttu-id="653bd-204">Hozzon létre hello kimeneti jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="653bd-204">Create hello output jar file.</span></span> <span data-ttu-id="653bd-205">Hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="653bd-205">Perform hello following steps.</span></span>

   1. <span data-ttu-id="653bd-206">A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** , majd a hello plusz jelre.</span><span class="sxs-lookup"><span data-stu-id="653bd-206">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="653bd-207">A hello előugró párbeszédpanelen kattintson az **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.</span><span class="sxs-lookup"><span data-stu-id="653bd-207">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>      
       
       <span data-ttu-id="653bd-208">![Apache Spark streamelési példa - JAR létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streamelési példa - JAR létrehozása")</span><span class="sxs-lookup"><span data-stu-id="653bd-208">![Apache Spark streaming example - create JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar.png "Apache Spark streaming example - create JAR")</span></span>
   2. <span data-ttu-id="653bd-209">A hello **modulokban létrehozása JAR** párbeszédpanel kattintson hello három pont (![három pont](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) hello elleni **fő osztály**.</span><span class="sxs-lookup"><span data-stu-id="653bd-209">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-eventhub-streaming/ellipsis.png)) against hello **Main Class**.</span></span>
   3. <span data-ttu-id="653bd-210">A hello **fő osztály kiválasztása** párbeszédpanel mezőben, válassza ki valamelyik hello elérhető osztályok, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="653bd-210">In hello **Select Main Class** dialog box, select any of hello available classes and then click **OK**.</span></span>
      
       <span data-ttu-id="653bd-211">![Apache Spark streaming példa - jar válassza osztály](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming példa - jar válassza osztály")</span><span class="sxs-lookup"><span data-stu-id="653bd-211">![Apache Spark streaming example - select class for jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-select-class-for-jar.png "Apache Spark streaming example - select class for jar")</span></span>
   4. <span data-ttu-id="653bd-212">A hello **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy hello beállítás túl**toohello cél JAR kibontása** van kiválasztva, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="653bd-212">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="653bd-213">Ez minden függőség egyetlen JAR hoz létre.</span><span class="sxs-lookup"><span data-stu-id="653bd-213">This creates a single JAR with all dependencies.</span></span>
      
       <span data-ttu-id="653bd-214">![Apache Spark streamelési példa - modulok jar létrehozása](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streamelési példa - modulok jar létrehozása")</span><span class="sxs-lookup"><span data-stu-id="653bd-214">![Apache Spark streaming example - create jar from modules](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-create-jar-from-modules.png "Apache Spark streaming example - create jar from modules")</span></span>
   5. <span data-ttu-id="653bd-215">Hello **kimeneti elrendezés** lap felsorolja az összes hello JAR-fájlok kivételével, amelyek tartalmazzák a hello Maven project.</span><span class="sxs-lookup"><span data-stu-id="653bd-215">hello **Output Layout** tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="653bd-216">Kiválaszthatja, és azokat, amelyeken hello Scala alkalmazás törlése hello nincs közvetlen függőség van.</span><span class="sxs-lookup"><span data-stu-id="653bd-216">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="653bd-217">Itt létrehozzuk hello alkalmazáshoz, akkor eltávolíthatja az összes, de az utolsó hello (**spark-adatfolyam-adatok-adatmegőrzési-példák fordítási kimeneti**).</span><span class="sxs-lookup"><span data-stu-id="653bd-217">For hello application we are creating here, you can remove all but hello last one (**spark-streaming-data-persistence-examples compile output**).</span></span> <span data-ttu-id="653bd-218">Hello JAR-fájlok kivételével toodelete válasszon, majd kattintson a hello **törlése** ikon (![törlés ikonja](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span><span class="sxs-lookup"><span data-stu-id="653bd-218">Select hello jars toodelete and then click hello **Delete** icon (![delete icon](./media/hdinsight-apache-spark-eventhub-streaming/delete-icon.png)).</span></span>
      
       <span data-ttu-id="653bd-219">![Apache Spark streaming példa - kibontott delete JAR-fájlok kivételével](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming - kibontott delete JAR-fájlok kivételével – példa")</span><span class="sxs-lookup"><span data-stu-id="653bd-219">![Apache Spark streaming example - delete extracted jars](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-delete-output-jars.png "Apache Spark streaming example - delete extracted jars")</span></span>
      
       <span data-ttu-id="653bd-220">Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy hello jar minden alkalommal létrejön hello projekt beépített vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="653bd-220">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="653bd-221">Kattintson az **Alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="653bd-221">Click **Apply**.</span></span>
   6. <span data-ttu-id="653bd-222">A hello **kimeneti elrendezés** lapon jobb alján hello hello **rendelkezésre álló elemek** mezőben van hello SQL JDBC jar, hogy hozzáadta a korábbi toohello projekt könyvtár.</span><span class="sxs-lookup"><span data-stu-id="653bd-222">In hello **Output Layout** tab, right at hello bottom of hello **Available Elements** box, you have hello SQL JDBC jar that you added earlier toohello project library.</span></span> <span data-ttu-id="653bd-223">Hozzá kell adni a toohello **kimeneti elrendezés** fülre. Kattintson a jobb gombbal a hello jar-fájlra, és kattintson **bontsa ki a kimeneti gyökér**.</span><span class="sxs-lookup"><span data-stu-id="653bd-223">You must add this toohello **Output Layout** tab. Right-click hello jar file, and then click **Extract Into Output Root**.</span></span>
      
       <span data-ttu-id="653bd-224">![Apache Spark streaming példa - kivonat függőségi jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming példa - kivonat függőségi jar")</span><span class="sxs-lookup"><span data-stu-id="653bd-224">![Apache Spark streaming example - extract dependency jar](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-extract-dependency-jar.png "Apache Spark streaming example - extract dependency jar")</span></span>  
      
       <span data-ttu-id="653bd-225">Hello **kimeneti elrendezés** lapon most példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="653bd-225">hello **Output Layout** tab should now look like this.</span></span>
      
       <span data-ttu-id="653bd-226">![Apache Spark streaming példa - végső kimenetet lapon](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming példa - végső kimenetet lap")</span><span class="sxs-lookup"><span data-stu-id="653bd-226">![Apache Spark streaming example - final output tab](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-final-output-tab.png "Apache Spark streaming example - final output tab")</span></span>        
      
       <span data-ttu-id="653bd-227">A hello **szerkezetének** párbeszédpanel, kattintson a **alkalmaz** majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="653bd-227">In hello **Project Structure** dialog box, click **Apply** and then click **OK**.</span></span>    
   7. <span data-ttu-id="653bd-228">Hello menüsorában kattintson **Build**, és kattintson a **ellenőrizze projekt**.</span><span class="sxs-lookup"><span data-stu-id="653bd-228">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="653bd-229">Is **Build összetevők** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="653bd-229">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="653bd-230">hello kimeneti jar alatt létrejön **\classes\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="653bd-230">hello output jar is created under **\classes\artifacts**.</span></span>
      
       <span data-ttu-id="653bd-231">![Apache Spark streamelési példa - kimeneti JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streamelési példa - kimeneti JAR")</span><span class="sxs-lookup"><span data-stu-id="653bd-231">![Apache Spark streaming example - output JAR](./media/hdinsight-apache-spark-eventhub-streaming/spark-streaming-example-output-jar.png "Apache Spark streaming example - output JAR")</span></span>

## <a name="run-hello-application-remotely-on-a-spark-cluster-using-livy"></a><span data-ttu-id="653bd-232">Hello alkalmazás távoli futtatása Spark-fürtön Livy használatával</span><span class="sxs-lookup"><span data-stu-id="653bd-232">Run hello application remotely on a Spark cluster using Livy</span></span>

<span data-ttu-id="653bd-233">Ez a cikk használhatja Livy toorun hello Apache Spark streamelési alkalmazás távolról Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="653bd-233">In this article you use Livy toorun hello Apache Spark streaming application remotely on a Spark cluster.</span></span> <span data-ttu-id="653bd-234">A hogyan toouse Livy HDInsight Spark a fürt részletes leírását lásd: [küldés feladatok távolról tooan Apache Spark fürtön Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-234">For detailed discussion on how toouse Livy with HDInsight Spark cluster, see [Submit jobs remotely tooan Apache Spark cluster on Azure HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span> <span data-ttu-id="653bd-235">Előtt hello Spark adatfolyam-továbbítási alkalmazást futtat, van néhány dolgot kell tennie:</span><span class="sxs-lookup"><span data-stu-id="653bd-235">Before you can start running hello Spark streaming application, there are a couple of things you should do:</span></span>

1. <span data-ttu-id="653bd-236">Indítsa el a hello helyi önálló toogenerate eseményeket, és tooEvent Hub küldött.</span><span class="sxs-lookup"><span data-stu-id="653bd-236">Start hello local standalone application toogenerate events and sent tooEvent Hub.</span></span> <span data-ttu-id="653bd-237">Ezért a következő parancs toodo hello használata:</span><span class="sxs-lookup"><span data-stu-id="653bd-237">Use hello following command toodo so:</span></span>

        java -cp com-microsoft-azure-eventhubs-client-example-0.2.0.jar com.microsoft.eventhubs.client.example.EventhubsClientDriver --eventhubs-namespace "mysbnamespace" --eventhubs-name "myeventhub" --policy-name "mysendpolicy" --policy-key "<policy key>" --message-length 32 --thread-count 32 --message-count -1

2. <span data-ttu-id="653bd-238">Másolás hello streaming jar (**spark-adatfolyam-adatok-adatmegőrzési-examples.jar**) toohello hello-fürthöz tartozó Azure Blob Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="653bd-238">Copy hello streaming jar (**spark-streaming-data-persistence-examples.jar**) toohello Azure Blob storage associated with hello cluster.</span></span> <span data-ttu-id="653bd-239">Így hello jar elérhető tooLivy.</span><span class="sxs-lookup"><span data-stu-id="653bd-239">This makes hello jar accessible tooLivy.</span></span> <span data-ttu-id="653bd-240">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancs így sor segédprogram, toodo.</span><span class="sxs-lookup"><span data-stu-id="653bd-240">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="653bd-241">Nincsenek sokkal más ügyfelek tooupload adatok használatával.</span><span class="sxs-lookup"><span data-stu-id="653bd-241">There are a lot of other clients you can use tooupload data.</span></span> <span data-ttu-id="653bd-242">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-242">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
3. <span data-ttu-id="653bd-243">Telepítse a CURL hello számítógépen az alkalmazások futtatása során.</span><span class="sxs-lookup"><span data-stu-id="653bd-243">Install CURL on hello computer where you are running these applications from.</span></span> <span data-ttu-id="653bd-244">CURL tooinvoke hello Livy végpontok toorun hello feladatok távolról használjuk.</span><span class="sxs-lookup"><span data-stu-id="653bd-244">We use CURL tooinvoke hello Livy endpoints toorun hello jobs remotely.</span></span>

### <a name="run-hello-spark-streaming-application-tooreceive-hello-events-into-an-azure-storage-blob-as-text"></a><span data-ttu-id="653bd-245">Egy Azure Storage-Blobba való hello Spark streamelési tooreceive hello alkalmazásesemények futtató szöveg</span><span class="sxs-lookup"><span data-stu-id="653bd-245">Run hello Spark streaming application tooreceive hello events into an Azure Storage Blob as text</span></span>

<span data-ttu-id="653bd-246">Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-246">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputBlob.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="653bd-247">hello fájlban paraméterek hello **inputBlob.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="653bd-247">hello parameters in hello file **inputBlob.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsEventCount", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="653bd-248">Ossza meg velünk megismeréséhez hello paraméterek hello bemeneti fájlban:</span><span class="sxs-lookup"><span data-stu-id="653bd-248">Let us understand what hello parameters in hello input file are:</span></span>

* <span data-ttu-id="653bd-249">**fájl** hello elérési toohello alkalmazás jar fájl hello hello-fürthöz tartozó Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="653bd-249">**file** is hello path toohello application jar file on hello Azure storage account associated with hello cluster.</span></span>
* <span data-ttu-id="653bd-250">**Osztálynév** hello hello jar hello osztály neve.</span><span class="sxs-lookup"><span data-stu-id="653bd-250">**className** is hello name of hello class in hello jar.</span></span>
* <span data-ttu-id="653bd-251">**argumentum** hello osztály kötelező argumentumok hello listája</span><span class="sxs-lookup"><span data-stu-id="653bd-251">**args** is hello list of arguments required by hello class</span></span>
* <span data-ttu-id="653bd-252">**numExecutors** mag, Spark toorun hello adatfolyam-alkalmazás által használt hello száma.</span><span class="sxs-lookup"><span data-stu-id="653bd-252">**numExecutors** is hello number of cores used by Spark toorun hello streaming application.</span></span> <span data-ttu-id="653bd-253">Mindig legyen az Event Hubs partíciók legalább két alkalommal hello száma.</span><span class="sxs-lookup"><span data-stu-id="653bd-253">This should always be at least twice hello number of Event Hub partitions.</span></span>
* <span data-ttu-id="653bd-254">**executorMemory**, **executorCores**, **driverMemory** használt paraméterek tooassign szükséges erőforrások toohello adatfolyam-továbbítási alkalmazások vannak.</span><span class="sxs-lookup"><span data-stu-id="653bd-254">**executorMemory**, **executorCores**, **driverMemory** are parameters used tooassign required resources toohello streaming application.</span></span>

> [!NOTE]
> <span data-ttu-id="653bd-255">Nem kell toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10), amelyek paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="653bd-255">You do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="653bd-256">adatfolyam-alkalmazás hello létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="653bd-256">hello streaming application creates them for you.</span></span>
>
>

<span data-ttu-id="653bd-257">Hello parancs futtatásakor hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="653bd-257">When you run hello command, you should see an output like hello following:</span></span>

    < HTTP/1.1 201 Created
    < Content-Type: application/json; charset=UTF-8
    < Location: /18
    < Server: Microsoft-IIS/8.5
    < X-Powered-By: ARR/2.5
    < X-Powered-By: ASP.NET
    < Date: Tue, 01 Dec 2015 05:39:10 GMT
    < Content-Length: 37
    <
    {"id":1,"state":"starting","log":[]}* Connection #0 toohost mysparkcluster.azurehdinsight.net left intact

<span data-ttu-id="653bd-258">Jegyezze fel a hello Kötegazonosító hello utolsó sorában hello kimeneti (a példában az értéke "1").</span><span class="sxs-lookup"><span data-stu-id="653bd-258">Make a note of hello batch ID in hello last line of hello output (in this example it is '1').</span></span> <span data-ttu-id="653bd-259">sikeresen lefutott, az alkalmazás hello tooverify, megtalálhatja a hello-fürthöz tartozó Azure-tárfiókot és hello kell megjelennie **/EventCount/EventCount10** létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="653bd-259">tooverify that hello application runs successfully, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventCount/EventCount10** folder created there.</span></span> <span data-ttu-id="653bd-260">Ebben a mappában kell tartalmaznia, amely rögzíti a megadott ideig tartó hello paraméter hello végrehajtva események hello száma blobok **batch-időköz-a-(másodperc)**.</span><span class="sxs-lookup"><span data-stu-id="653bd-260">This folder should contain blobs that captures hello number of events processed within hello time period specified for hello parameter **batch-interval-in-seconds**.</span></span>

<span data-ttu-id="653bd-261">hello Spark streamelési alkalmazás toorun folytatódik, amíg állítsa le.</span><span class="sxs-lookup"><span data-stu-id="653bd-261">hello Spark streaming application will continue toorun until you kill it.</span></span> <span data-ttu-id="653bd-262">toodo Igen, használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-262">toodo so, use hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -X DELETE "https://mysparkcluster.azurehdinsight.net/livy/batches/1"

### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-storage-blob-as-json"></a><span data-ttu-id="653bd-263">Hello alkalmazások futtatásához tooreceive hello eseményeket az Azure Storage-Blobba JSON-fájlként</span><span class="sxs-lookup"><span data-stu-id="653bd-263">Run hello applications tooreceive hello events into an Azure Storage Blob as JSON</span></span>
<span data-ttu-id="653bd-264">Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-264">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputJSON.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="653bd-265">hello fájlban paraméterek hello **inputJSON.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="653bd-265">hello parameters in hello file **inputJSON.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureBlobAsJSON", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-store-folder", "/EventStore10"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="653bd-266">hello paraméterei hello szöveges kimenet hello előző lépésben megadott hasonló toowhat.</span><span class="sxs-lookup"><span data-stu-id="653bd-266">hello parameters are similar toowhat you specified for hello text output, in hello previous step.</span></span> <span data-ttu-id="653bd-267">Ebben az esetben nem kell toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10), amelyek paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="653bd-267">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) that are used as parameters.</span></span> <span data-ttu-id="653bd-268">adatfolyam-alkalmazás hello létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="653bd-268">hello streaming application creates them for you.</span></span>

 <span data-ttu-id="653bd-269">Miután hello parancs futtatása, megtalálhatja a hello-fürthöz tartozó Azure-tárfiók és hello kell megjelennie **/EventStore10** létrehozott mappába.</span><span class="sxs-lookup"><span data-stu-id="653bd-269">After you run hello command, you can look at your Azure storage account associated with hello cluster and you should see hello **/EventStore10** folder created there.</span></span> <span data-ttu-id="653bd-270">Nyissa meg a fájl a következő előtaggal **rész -** és hello események feldolgozása JSON formátumban kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="653bd-270">Open any file prefixed with **part-** and you should see hello events processed in a JSON format.</span></span>

### <a name="run-hello-applications-tooreceive-hello-events-into-a-hive-table"></a><span data-ttu-id="653bd-271">A Hive tábla tooreceive hello események hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="653bd-271">Run hello applications tooreceive hello events into a Hive table</span></span>
<span data-ttu-id="653bd-272">toorun hello adatfolyamok események egy Hive tábla, Spark streamelési alkalmazás kell néhány további összetevőket.</span><span class="sxs-lookup"><span data-stu-id="653bd-272">toorun hello Spark streaming application that streams events into a Hive table you need some additional components.</span></span> <span data-ttu-id="653bd-273">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="653bd-273">These are:</span></span>

* <span data-ttu-id="653bd-274">datanucleus-api-jdo-3.2.6.jar</span><span class="sxs-lookup"><span data-stu-id="653bd-274">datanucleus-api-jdo-3.2.6.jar</span></span>
* <span data-ttu-id="653bd-275">datanucleus-rdbms-3.2.9.jar</span><span class="sxs-lookup"><span data-stu-id="653bd-275">datanucleus-rdbms-3.2.9.jar</span></span>
* <span data-ttu-id="653bd-276">datanucleus-core-3.2.10.jar</span><span class="sxs-lookup"><span data-stu-id="653bd-276">datanucleus-core-3.2.10.jar</span></span>
* <span data-ttu-id="653bd-277">Hive-site.xml</span><span class="sxs-lookup"><span data-stu-id="653bd-277">hive-site.xml</span></span>

<span data-ttu-id="653bd-278">Hello **.jar** fájlok érhetők el: a HDInsight Spark-fürt `/usr/hdp/current/spark-client/lib`.</span><span class="sxs-lookup"><span data-stu-id="653bd-278">hello **.jar** files are available on your HDInsight Spark cluster at `/usr/hdp/current/spark-client/lib`.</span></span> <span data-ttu-id="653bd-279">Hello **hive-site.xml** érhető el `/usr/hdp/current/spark-client/conf`.</span><span class="sxs-lookup"><span data-stu-id="653bd-279">hello **hive-site.xml** is available at `/usr/hdp/current/spark-client/conf`.</span></span>

<span data-ttu-id="653bd-280">Használhat [WinScp](http://winscp.net/eng/download.php) toocopy ezeket a fájlokat hello fürt tooyour helyi számítógépről.</span><span class="sxs-lookup"><span data-stu-id="653bd-280">You can use [WinScp](http://winscp.net/eng/download.php) toocopy over these files from hello cluster tooyour local computer.</span></span> <span data-ttu-id="653bd-281">Ezek a fájlok tooyour tárfiók keresztül hello-fürthöz tartozó eszközök toocopy használhatja.</span><span class="sxs-lookup"><span data-stu-id="653bd-281">You can then use tools toocopy these files over tooyour storage account associated with hello cluster.</span></span> <span data-ttu-id="653bd-282">Hogyan tooupload fájlok toohello tárfiók további információkért lásd: [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-282">For more information on how tooupload files toohello storage account, see [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>

<span data-ttu-id="653bd-283">Miután átmásolta a keresztül hello fájlok tooyour Azure storage-fiók, nyisson meg egy parancssort, keresse meg a CURL telepítési toohello directory és futtassa a következő parancsot (a név felülírandó felhasználónév/jelszó és a fürt nevét) hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-283">Once you have copied over hello files tooyour Azure storage account, open a command prompt, navigate toohello directory where you installed CURL, and run hello following command (replace username/password and cluster name):</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputHive.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="653bd-284">hello fájlban paraméterek hello **inputHive.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="653bd-284">hello parameters in hello file **inputHive.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToHiveTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--event-hive-table", "EventHiveTable10" ], "jars":["wasb:///example/jars/datanucleus-api-jdo-3.2.6.jar", "wasb:///example/jars/datanucleus-rdbms-3.2.9.jar", "wasb:///example/jars/datanucleus-core-3.2.10.jar"], "files":["wasb:///example/jars/hive-site.xml"], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="653bd-285">hello paraméterei hello szöveges kimenet hello előző lépésben megadott hasonló toowhat.</span><span class="sxs-lookup"><span data-stu-id="653bd-285">hello parameters are similar toowhat you specified for hello text output, in hello previous steps.</span></span> <span data-ttu-id="653bd-286">Ebben az esetben nincs szükség toocreate hello kimeneti mappák (EventCheckpoint, EventCount/EventCount10) vagy hello kimeneti paraméterként használt Hive tábla (EventHiveTable10).</span><span class="sxs-lookup"><span data-stu-id="653bd-286">Again, you do not need toocreate hello output folders (EventCheckpoint, EventCount/EventCount10) or hello output Hive table (EventHiveTable10) that are used as parameters.</span></span> <span data-ttu-id="653bd-287">adatfolyam-alkalmazás hello létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="653bd-287">hello streaming application creates them for you.</span></span> <span data-ttu-id="653bd-288">Vegye figyelembe, hogy hello **JAR-fájlok kivételével** és **fájlok** beállítás elérési utak toohello .jar fájlok és hello hive-site.xml toohello tárfiók keresztül másolt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="653bd-288">Note that hello **jars** and **files** option includes paths toohello .jar files and hello hive-site.xml that you copied over toohello storage account.</span></span>

<span data-ttu-id="653bd-289">SSH alkalmas hello fürt és a Hive-lekérdezések futtatása, tooverify, amely hello hive tábla létrehozása sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="653bd-289">tooverify that hello hive table was successfully created, you can SSH into hello cluster and run Hive queries.</span></span> <span data-ttu-id="653bd-290">Útmutatásért lásd: [használja az SSH a HDInsight Hadoop Hive](hdinsight-hadoop-use-hive-ssh.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-290">For instructions, see [Use Hive with Hadoop in HDInsight with SSH](hdinsight-hadoop-use-hive-ssh.md).</span></span> <span data-ttu-id="653bd-291">Miután csatlakozott SSH használatával, futtathatja a következő parancs tooverify hello hello Hive táblát, **EventHiveTable10**, jön létre.</span><span class="sxs-lookup"><span data-stu-id="653bd-291">Once you are connected using SSH, you can run hello following command tooverify that hello Hive table, **EventHiveTable10**, is created.</span></span>

    show tables;

<span data-ttu-id="653bd-292">Egy kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="653bd-292">You should see an output similar toohello following:</span></span>

    OK
    eventhivetable10
    hivesampletable

<span data-ttu-id="653bd-293">A SELECT lekérdezés hello tábla tooview hello tartalmát is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="653bd-293">You can also run a SELECT query tooview hello contents of hello table.</span></span>

    SELECT * FROM eventhivetable10 LIMIT 10;

<span data-ttu-id="653bd-294">Hello hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="653bd-294">You should see an output like hello following:</span></span>

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


### <a name="run-hello-applications-tooreceive-hello-events-into-an-azure-sql-database-table"></a><span data-ttu-id="653bd-295">Egy Azure SQL adatbázis táblába tooreceive hello események hello alkalmazások futtatásához</span><span class="sxs-lookup"><span data-stu-id="653bd-295">Run hello applications tooreceive hello events into an Azure SQL database table</span></span>
<span data-ttu-id="653bd-296">Ez a lépés futtatása előtt győződjön meg arról, hogy az Azure SQL-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="653bd-296">Before running this step, make sure you have an Azure SQL database created.</span></span> <span data-ttu-id="653bd-297">Útmutatásért lásd: [SQL-adatbázis létrehozása percben](../sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-297">For instructions, see [Create a SQL database in minutes](../sql-database/sql-database-get-started.md).</span></span> <span data-ttu-id="653bd-298">toocomplete Ez részben, értékek adatbázisnév, adatbázis-kiszolgáló nevét és hello adatbázis rendszergazdai hitelesítő adatok paraméterként van szüksége.</span><span class="sxs-lookup"><span data-stu-id="653bd-298">toocomplete this section, you need values for database name, database server name, and hello database administrator credentials as parameters.</span></span> <span data-ttu-id="653bd-299">Toocreate hello adatbázistábla azonban nem kell.</span><span class="sxs-lookup"><span data-stu-id="653bd-299">You do not need toocreate hello database table though.</span></span> <span data-ttu-id="653bd-300">hello Spark adatfolyam-továbbítási alkalmazást hoz létre, amely meg.</span><span class="sxs-lookup"><span data-stu-id="653bd-300">hello Spark streaming application creates that for you.</span></span>

<span data-ttu-id="653bd-301">Nyisson meg egy parancssort, keresse meg a toohello könyvtárba CURL telepítette, és futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-301">Open a command prompt, navigate toohello directory where you installed CURL, and run hello following command:</span></span>

    curl -k --user "admin:mypassword1!" -v -H "Content-Type: application/json" -X POST --data @C:\Temp\inputSQL.txt "https://mysparkcluster.azurehdinsight.net/livy/batches"

<span data-ttu-id="653bd-302">hello fájlban paraméterek hello **inputSQL.txt** az alábbiak szerint definiáltuk:</span><span class="sxs-lookup"><span data-stu-id="653bd-302">hello parameters in hello file **inputSQL.txt** are defined as follows:</span></span>

    { "file":"wasb:///example/jars/spark-streaming-data-persistence-examples.jar", "className":"com.microsoft.spark.streaming.examples.workloads.EventhubsToAzureSQLTable", "args":["--eventhubs-namespace", "mysbnamespace", "--eventhubs-name", "myeventhub", "--policy-name", "myreceivepolicy", "--policy-key", "<put-your-key-here>", "--consumer-group", "$default", "--partition-count", 10, "--batch-interval-in-seconds", 20, "--checkpoint-directory", "/EventCheckpoint", "--event-count-folder", "/EventCount/EventCount10", "--sql-server-fqdn", "<database-server-name>.database.windows.net", "--sql-database-name", "mysparkdatabase", "--database-username", "sparkdbadmin", "--database-password", "<put-password-here>", "--event-sql-table", "EventContent" ], "numExecutors":20, "executorMemory":"1G", "executorCores":1, "driverMemory":"2G" }

<span data-ttu-id="653bd-303">sikeresen lefutott, az alkalmazás hello tooverify, toohello Azure SQL-adatbázis SQL Server Management Studio használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="653bd-303">tooverify that hello application runs successfully, you can connect toohello Azure SQL database using SQL Server Management Studio.</span></span> <span data-ttu-id="653bd-304">Útmutatást, lásd: toodo [tooSQL adatbázis csatlakozzon az SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="653bd-304">For instructions on how toodo that, see [Connect tooSQL Database with SQL Server Management Studio](../sql-database/sql-database-connect-query-ssms.md).</span></span> <span data-ttu-id="653bd-305">Ha csatlakoztatott toohello adatbázis, navigálhat toohello **EventContent** hello adatfolyam-alkalmazás által létrehozott tábla.</span><span class="sxs-lookup"><span data-stu-id="653bd-305">Once you are connected toohello database, you can navigate toohello **EventContent** table that was created by hello streaming application.</span></span> <span data-ttu-id="653bd-306">Hello táblából futtatása egy gyors tooget hello adatait kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="653bd-306">You can run a quick query tooget hello data from hello table.</span></span> <span data-ttu-id="653bd-307">Futtassa a következő lekérdezés hello:</span><span class="sxs-lookup"><span data-stu-id="653bd-307">Run hello following query:</span></span>

    SELECT * FROM EventCount

<span data-ttu-id="653bd-308">Kimeneti hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="653bd-308">You should see output similar toohello following:</span></span>

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


## <span data-ttu-id="653bd-309"><a name="seealso"></a>Lásd még:</span><span class="sxs-lookup"><span data-stu-id="653bd-309"><a name="seealso"></a>See also</span></span>
* [<span data-ttu-id="653bd-310">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="653bd-310">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)
* [<span data-ttu-id="653bd-311">Fogadó-alapú kapcsolat és a közvetlen DStream tervezése</span><span class="sxs-lookup"><span data-stu-id="653bd-311">Design of Receiver-based Connection and Direct DStream</span></span>](https://www.slideshare.net/NanZhu/seattle-sparkmeetup032317)

### <a name="scenarios"></a><span data-ttu-id="653bd-312">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="653bd-312">Scenarios</span></span>
* [<span data-ttu-id="653bd-313">Spark és BI: Interaktív adatelemzés végrehajtása a Spark on HDInsight használatával, BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="653bd-313">Spark with BI: Perform interactive data analysis using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="653bd-314">Spark és Machine Learning: A Spark on HDInsight használata az épület-hőmérséklet elemzésére HVAC-adatok alapján</span><span class="sxs-lookup"><span data-stu-id="653bd-314">Spark with Machine Learning: Use Spark in HDInsight for analyzing building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="653bd-315">Spark és Machine Learning: használja a Spark on HDInsight toopredict élelmiszervizsgálati eredmények</span><span class="sxs-lookup"><span data-stu-id="653bd-315">Spark with Machine Learning: Use Spark in HDInsight toopredict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="653bd-316">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="653bd-316">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a><span data-ttu-id="653bd-317">Alkalmazások létrehozása és futtatása</span><span class="sxs-lookup"><span data-stu-id="653bd-317">Create and run applications</span></span>
* [<span data-ttu-id="653bd-318">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="653bd-318">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="653bd-319">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="653bd-319">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="653bd-320">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="653bd-320">Tools and extensions</span></span>
* [<span data-ttu-id="653bd-321">Toocreate IntelliJ IDEA HDInsight-eszközei beépülő használja, és küldje el a Spark Scala applicatons</span><span class="sxs-lookup"><span data-stu-id="653bd-321">Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark Scala applicatons</span></span>](hdinsight-apache-spark-intellij-tool-plugin.md)
* [<span data-ttu-id="653bd-322">IntelliJ IDEA toodebug Spark-alkalmazások HDInsight-eszközei beépülő távolról használni</span><span class="sxs-lookup"><span data-stu-id="653bd-322">Use HDInsight Tools Plugin for IntelliJ IDEA toodebug Spark applications remotely</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="653bd-323">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="653bd-323">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="653bd-324">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="653bd-324">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="653bd-325">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="653bd-325">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="653bd-326">Jupyter telepítse a számítógépre, és csatlakozzon a HDInsight Spark-fürt tooan</span><span class="sxs-lookup"><span data-stu-id="653bd-326">Install Jupyter on your computer and connect tooan HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a><span data-ttu-id="653bd-327">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="653bd-327">Manage resources</span></span>
* [<span data-ttu-id="653bd-328">Az Azure HDInsight hello Apache Spark-fürt erőforrásainak kezelése</span><span class="sxs-lookup"><span data-stu-id="653bd-328">Manage resources for hello Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="653bd-329">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="653bd-329">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-storage]: hdinsight-hadoop-use-blob-storage.md

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-create-storageaccount]: ../storage-create-storage-account/
