---
title: "Az Event Hubs Storm - Azure HDInsight származó események feldolgozására |} Microsoft Docs"
description: "Ismerje meg, hogyan kell feldolgozni az adatokat az Azure Event Hubs egy C# Storm-topológia hozta létre a Visual Studio, a HDInsight tools for Visual Studio használatával."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 67f9d08c-eea0-401b-952b-db765655dad0
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.openlocfilehash: 4b6fd87b057d93175d3ef284238d77be3bdde61d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="c6ced-103">Az Azure Event Hubs (C#) futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="c6ced-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="c6ced-104">Megismerheti az Azure Event Hubs a HDInsight alatt futó Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="c6ced-104">Learn how to work with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="c6ced-105">Ez a dokumentum olvasása és írása az adatok Evbent hubs használja a C# Storm-topológia</span><span class="sxs-lookup"><span data-stu-id="c6ced-105">This document uses a C# Storm topology to read and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="c6ced-106">Ez a projekt Java verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="c6ced-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="c6ced-107">SCP.NET</span></span>

<span data-ttu-id="c6ced-108">A jelen dokumentumban leírt lépések SCP.NET, a NuGet-csomagot, amely megkönnyíti a HDInsight alatt futó Storm a C#-topológiák és használatra összetevők létrehozásához használja.</span><span class="sxs-lookup"><span data-stu-id="c6ced-108">The steps in this document use SCP.NET, a NuGet package that makes it easy to create C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6ced-109">A jelen dokumentumban leírt lépések a Visual Studio Windows fejlesztői környezetre támaszkodnak, amíg a lefordított projekt küldheti el Storm on HDInsight-fürt által használt Linux.</span><span class="sxs-lookup"><span data-stu-id="c6ced-109">While the steps in this document rely on a Windows development environment with Visual Studio, the compiled project can be submitted to a Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="c6ced-110">Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.</span><span class="sxs-lookup"><span data-stu-id="c6ced-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="c6ced-111">HDInsight 3.4 és nagyobb használata monó C#-topológiák futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c6ced-111">HDInsight 3.4 and greater use Mono to run C# topologies.</span></span> <span data-ttu-id="c6ced-112">Ebben a dokumentumban bemutatott példában a HDInsight 3.6 működik.</span><span class="sxs-lookup"><span data-stu-id="c6ced-112">The example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="c6ced-113">Ha a HDInsight a saját .NET megoldások létrehozását tervezi, akkor ellenőrizze a [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.</span><span class="sxs-lookup"><span data-stu-id="c6ced-113">If you plan on creating your own .NET solutions for HDInsight, check the [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="c6ced-114">Fürt versioning</span><span class="sxs-lookup"><span data-stu-id="c6ced-114">Cluster versioning</span></span>

<span data-ttu-id="c6ced-115">A Microsoft.SCP.Net.SDK NuGet-csomagot a projekthez használt telepítve a HDInsight alatt futó Storm főverziója egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="c6ced-115">The Microsoft.SCP.Net.SDK NuGet package you use for your project must match the major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="c6ced-116">HDInsight-verziókról 3.5-ös és 3.6 alatt futó Stormot használni 1.x, így ezeken a fürtökön SCP.NET verzió 1.0.x.x kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c6ced-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6ced-117">Ebben a dokumentumban a példa egy HDInsight 3.5-ös vagy 3,6 fürt vár.</span><span class="sxs-lookup"><span data-stu-id="c6ced-117">The example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="c6ced-118">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="c6ced-118">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c6ced-119">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c6ced-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="c6ced-120">C#-topológiák is kell használnia célként .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c6ced-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-to-work-with-event-hubs"></a><span data-ttu-id="c6ced-121">Az Event Hubs használata</span><span class="sxs-lookup"><span data-stu-id="c6ced-121">How to work with Event Hubs</span></span>

<span data-ttu-id="c6ced-122">Microsoft biztosít, amelyek segítségével kommunikál az Event Hubs egy Storm-topológia a Java-összetevők.</span><span class="sxs-lookup"><span data-stu-id="c6ced-122">Microsoft provides a set of Java components that can be used to communicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="c6ced-123">A Java-archívumfájl (JAR), amely tartalmazza ezeket az összetevőket, egy HDInsight 3.6 kompatibilis verziója található [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="c6ced-123">You can find the Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c6ced-124">Amíg az összetevők Java nyelven íródtak, könnyen használhatja őket a C#-topológiák.</span><span class="sxs-lookup"><span data-stu-id="c6ced-124">While the components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="c6ced-125">A következő összetevők szerepelnek ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="c6ced-125">The following components are used in this example:</span></span>

* <span data-ttu-id="c6ced-126">__EventHubSpout__: olvassa be az adatokat az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="c6ced-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="c6ced-127">__EventHubBolt__: az Event Hubs írja az adatokat.</span><span class="sxs-lookup"><span data-stu-id="c6ced-127">__EventHubBolt__: Writes data to Event Hubs.</span></span>
* <span data-ttu-id="c6ced-128">__EventHubSpoutConfig__: EventHubSpout konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c6ced-128">__EventHubSpoutConfig__: Used to configure EventHubSpout.</span></span>
* <span data-ttu-id="c6ced-129">__EventHubBoltConfig__: EventHubBolt konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c6ced-129">__EventHubBoltConfig__: Used to configure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="c6ced-130">Spout-használat – példa</span><span class="sxs-lookup"><span data-stu-id="c6ced-130">Example spout usage</span></span>

<span data-ttu-id="c6ced-131">SCP.NET egy EventHubSpout ad hozzá a topológia módszereket kínál.</span><span class="sxs-lookup"><span data-stu-id="c6ced-131">SCP.NET provides methods for adding an EventHubSpout to your topology.</span></span> <span data-ttu-id="c6ced-132">Ezek a módszerek könnyebben egy spout, mint az általános metódusok használata egy Java-összetevő felvételéhez adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="c6ced-132">These methods make it easier to add a spout than using the generic methods for adding a Java component.</span></span> <span data-ttu-id="c6ced-133">A következő példa bemutatja, hogyan hozhat létre egy spout használatával a __SetEventHubSpout__ és **EventHubSpoutConfig** SCP.NET által biztosított módszerek:</span><span class="sxs-lookup"><span data-stu-id="c6ced-133">The following example demonstrates how to create a spout by using the __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

```csharp
 topologyBuilder.SetEventHubSpout(
    "EventHubSpout",
    new EventHubSpoutConfig(
        ConfigurationManager.AppSettings["EventHubSharedAccessKeyName"],
        ConfigurationManager.AppSettings["EventHubSharedAccessKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        ConfigurationManager.AppSettings["EventHubEntityPath"],
        eventHubPartitions),
    eventHubPartitions);
```

<span data-ttu-id="c6ced-134">Az előző példában hoz létre egy új nevű spout összetevő __EventHubSpout__, és konfigurálja az eseményközpontok folytatott kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="c6ced-134">The previous example creates a new spout component named __EventHubSpout__, and configures it to communicate with an event hub.</span></span> <span data-ttu-id="c6ced-135">Az összetevő a párhuzamos végrehajtás mutató értéke a partíciók számának az Eseménynapló hub.</span><span class="sxs-lookup"><span data-stu-id="c6ced-135">The parallelism hint for the component is set to the number of partitions in the event hub.</span></span> <span data-ttu-id="c6ced-136">Ez a beállítás lehetővé teszi, hogy a Storm minden partíció esetében összetevő példányának létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c6ced-136">This setting allows Storm to create an instance of the component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="c6ced-137">Példa a bolt használatra</span><span class="sxs-lookup"><span data-stu-id="c6ced-137">Example bolt usage</span></span>

<span data-ttu-id="c6ced-138">Használja a **JavaComponmentConstructor** metódust tartalmaz a bolt egy példányának létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="c6ced-138">Use the **JavaComponmentConstructor** method to create an instance of the bolt.</span></span> <span data-ttu-id="c6ced-139">A következő példa bemutatja, hogyan hozza létre és konfigurálja egy új példányát a **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="c6ced-139">The following example demonstrates how to create and configure a new instance of the **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for the Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set the bolt to subscribe to data from the spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="c6ced-140">Ez a példa egy karakterláncként használata helyett átadott Clojure kifejezés **JavaComponentConstructor** létrehozásához egy **EventHubBoltConfig**, mint a spout példa.</span><span class="sxs-lookup"><span data-stu-id="c6ced-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** to create an **EventHubBoltConfig**, as the spout example did.</span></span> <span data-ttu-id="c6ced-141">Mindkét módszer használható.</span><span class="sxs-lookup"><span data-stu-id="c6ced-141">Either method works.</span></span> <span data-ttu-id="c6ced-142">Érzi, a legjobb módszert használja.</span><span class="sxs-lookup"><span data-stu-id="c6ced-142">Use the method that feels best to you.</span></span>

## <a name="download-the-completed-project"></a><span data-ttu-id="c6ced-143">A befejezett projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="c6ced-143">Download the completed project</span></span>

<span data-ttu-id="c6ced-144">Ebben az oktatóanyagban a létrehozott projekt teljes verziója letölthető [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="c6ced-144">You can download a complete version of the project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="c6ced-145">Azonban továbbra is szeretné az oktatóanyag lépéseit követve adja meg a konfigurációs beállításokat.</span><span class="sxs-lookup"><span data-stu-id="c6ced-145">However, you still need to provide configuration settings by following the steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="c6ced-146">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c6ced-146">Prerequisites</span></span>

* <span data-ttu-id="c6ced-147">Egy [Apache Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="c6ced-148">Ebben a dokumentumban bemutatott példában alatt futó Storm példatopológiái 3.5-ös vagy 3.6 verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="c6ced-148">The example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="c6ced-149">Ez miatt nem működik a HDInsight, korábbi verzióival való megtörje osztály neve megváltozik.</span><span class="sxs-lookup"><span data-stu-id="c6ced-149">This does not work with older versions of HDInsight, due to breaking class name changes.</span></span> <span data-ttu-id="c6ced-150">Ebben a példában, amely kompatibilis a régebbi fürtök verziója: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="c6ced-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="c6ced-151">Egy [Azure event hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="c6ced-152">A [az Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="c6ced-152">The [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="c6ced-153">A [a HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-153">The [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="c6ced-154">Java JDK 1.8 vagy később a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="c6ced-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="c6ced-155">JDK letöltések érhetők el a [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="c6ced-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="c6ced-156">A **JAVA_HOME** környezeti változó Java tartalmazó könyvtárat kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="c6ced-156">The **JAVA_HOME** environment variable must point to the directory that contains Java.</span></span>
  * <span data-ttu-id="c6ced-157">A **%JAVA_HOME%/bin** könyvtárnak az elérési úton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c6ced-157">The **%JAVA_HOME%/bin** directory must be in the path.</span></span>

## <a name="download-the-event-hubs-components"></a><span data-ttu-id="c6ced-158">Az Event Hubs összetevők letöltése</span><span class="sxs-lookup"><span data-stu-id="c6ced-158">Download the Event Hubs components</span></span>

<span data-ttu-id="c6ced-159">Töltse le az Event Hubs spout, és az összetevőt erről a boltok [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="c6ced-159">Download the Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="c6ced-160">Hozzon létre egy könyvtárat nevű `eventhubspout`, és mentse a fájlt a könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c6ced-160">Create a directory named `eventhubspout`, and save the file into the directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="c6ced-161">Az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6ced-161">Configure Event Hubs</span></span>

<span data-ttu-id="c6ced-162">Az Event Hubs ebben a példában az adatforrást.</span><span class="sxs-lookup"><span data-stu-id="c6ced-162">Event Hubs is the data source for this example.</span></span> <span data-ttu-id="c6ced-163">A "Létrehoz egy eseményközpontot" szakaszában foglaltak [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-163">Use the information in the "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="c6ced-164">Az eseményközpont létrehozása után megtekintheti a **EventHub** panel az Azure portál, majd válassza az **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-164">After the event hub has been created, view the **EventHub** blade in the Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="c6ced-165">Válassza ki **+ Hozzáadás** a következő házirendek hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="c6ced-165">Select **+ Add** to add the following policies:</span></span>

   | <span data-ttu-id="c6ced-166">Név</span><span class="sxs-lookup"><span data-stu-id="c6ced-166">Name</span></span> | <span data-ttu-id="c6ced-167">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="c6ced-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6ced-168">író</span><span class="sxs-lookup"><span data-stu-id="c6ced-168">writer</span></span> |<span data-ttu-id="c6ced-169">Küldés</span><span class="sxs-lookup"><span data-stu-id="c6ced-169">Send</span></span> |
   | <span data-ttu-id="c6ced-170">olvasó</span><span class="sxs-lookup"><span data-stu-id="c6ced-170">reader</span></span> |<span data-ttu-id="c6ced-171">Figyelés</span><span class="sxs-lookup"><span data-stu-id="c6ced-171">Listen</span></span> |

    ![Képernyőkép a fájlmegosztás hozzáférési házirendek ablak](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="c6ced-173">Válassza ki a **olvasó** és **író** házirendek.</span><span class="sxs-lookup"><span data-stu-id="c6ced-173">Select the **reader** and **writer** policies.</span></span> <span data-ttu-id="c6ced-174">Másolja ki és mentse a házirendet, az elsődleges kulcs értéke később használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c6ced-174">Copy and save the primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-the-eventhubwriter"></a><span data-ttu-id="c6ced-175">A EventHubWriter konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6ced-175">Configure the EventHubWriter</span></span>

1. <span data-ttu-id="c6ced-176">Ha még nem telepítette a legújabb verzióját a HDInsight tools for Visual Studio, lásd: [első lépései a HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="c6ced-176">If you have not already installed the latest version of the HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="c6ced-177">Letöltheti a megoldást a [eventhub-storm-hibrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="c6ced-177">Download the solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="c6ced-178">Az a **EventHubWriter** projektben nyissa meg a **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6ced-178">In the **EventHubWriter** project, open the **App.config** file.</span></span> <span data-ttu-id="c6ced-179">Olvassa el az eseményközpontból korábban megadott értékektől töltse ki a következő kulcs értéke:</span><span class="sxs-lookup"><span data-stu-id="c6ced-179">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="c6ced-180">Kulcs</span><span class="sxs-lookup"><span data-stu-id="c6ced-180">Key</span></span> | <span data-ttu-id="c6ced-181">Érték</span><span class="sxs-lookup"><span data-stu-id="c6ced-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6ced-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="c6ced-182">EventHubPolicyName</span></span> |<span data-ttu-id="c6ced-183">író (Ha használja-e egy másik nevet a házirend *küldése* engedéllyel, használja helyette.)</span><span class="sxs-lookup"><span data-stu-id="c6ced-183">writer (If you used a different name for the policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="c6ced-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="c6ced-184">EventHubPolicyKey</span></span> |<span data-ttu-id="c6ced-185">Az író házirend kulcsa.</span><span class="sxs-lookup"><span data-stu-id="c6ced-185">The key for the writer policy.</span></span> |
   | <span data-ttu-id="c6ced-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="c6ced-186">EventHubNamespace</span></span> |<span data-ttu-id="c6ced-187">A névtér, amely tartalmazza az eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="c6ced-187">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="c6ced-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="c6ced-188">EventHubName</span></span> |<span data-ttu-id="c6ced-189">Az eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="c6ced-189">Your event hub name.</span></span> |
   | <span data-ttu-id="c6ced-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="c6ced-190">EventHubPartitionCount</span></span> |<span data-ttu-id="c6ced-191">A partíciók az eseményközpont száma.</span><span class="sxs-lookup"><span data-stu-id="c6ced-191">The number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="c6ced-192">Mentse és zárja be a **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6ced-192">Save and close the **App.config** file.</span></span>

## <a name="configure-the-eventhubreader"></a><span data-ttu-id="c6ced-193">A EventHubReader konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c6ced-193">Configure the EventHubReader</span></span>

1. <span data-ttu-id="c6ced-194">Nyissa meg a **EventHubReader** projekt.</span><span class="sxs-lookup"><span data-stu-id="c6ced-194">Open the **EventHubReader** project.</span></span>

2. <span data-ttu-id="c6ced-195">Nyissa meg a **App.config** fájlt a **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-195">Open the **App.config** file for the **EventHubReader**.</span></span> <span data-ttu-id="c6ced-196">Olvassa el az eseményközpontból korábban megadott értékektől töltse ki a következő kulcs értéke:</span><span class="sxs-lookup"><span data-stu-id="c6ced-196">Use the information from the event hub that you configured earlier to fill in the value for the following keys:</span></span>

   | <span data-ttu-id="c6ced-197">Kulcs</span><span class="sxs-lookup"><span data-stu-id="c6ced-197">Key</span></span> | <span data-ttu-id="c6ced-198">Érték</span><span class="sxs-lookup"><span data-stu-id="c6ced-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="c6ced-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="c6ced-199">EventHubPolicyName</span></span> |<span data-ttu-id="c6ced-200">olvasó (Ha használja-e egy másik nevet a házirend *figyelésére* engedéllyel, használja helyette.)</span><span class="sxs-lookup"><span data-stu-id="c6ced-200">reader (If you used a different name for the policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="c6ced-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="c6ced-201">EventHubPolicyKey</span></span> |<span data-ttu-id="c6ced-202">A kulcs az olvasó házirend.</span><span class="sxs-lookup"><span data-stu-id="c6ced-202">The key for the reader policy.</span></span> |
   | <span data-ttu-id="c6ced-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="c6ced-203">EventHubNamespace</span></span> |<span data-ttu-id="c6ced-204">A névtér, amely tartalmazza az eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="c6ced-204">The namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="c6ced-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="c6ced-205">EventHubName</span></span> |<span data-ttu-id="c6ced-206">Az eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="c6ced-206">Your event hub name.</span></span> |
   | <span data-ttu-id="c6ced-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="c6ced-207">EventHubPartitionCount</span></span> |<span data-ttu-id="c6ced-208">A partíciók az eseményközpont száma.</span><span class="sxs-lookup"><span data-stu-id="c6ced-208">The number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="c6ced-209">Mentse és zárja be a **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="c6ced-209">Save and close the **App.config** file.</span></span>

## <a name="deploy-the-topologies"></a><span data-ttu-id="c6ced-210">A topológiák telepítése</span><span class="sxs-lookup"><span data-stu-id="c6ced-210">Deploy the topologies</span></span>

1. <span data-ttu-id="c6ced-211">A **Megoldáskezelőben**, kattintson a jobb gombbal a **EventHubReader** projektre, és válassza a **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-211">From **Solution Explorer**, right-click the **EventHubReader** project, and select **Submit to Storm on HDInsight**.</span></span>

    ![Képernyőfelvétel a Solution Explorer, a kiemelt HDInsight alatt futó Storm való küldés](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="c6ced-213">Az a **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-213">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="c6ced-214">Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és válassza ki a korábban letöltött JAR-fájlt tartalmazó könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c6ced-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file that you downloaded earlier.</span></span> <span data-ttu-id="c6ced-215">Végezetül kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-215">Finally, click **Submit**.</span></span>

    ![Topológia nyújt képernyőkép párbeszédpanel](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="c6ced-217">A topológia elküldésekor a **Storm-topológiák Viewer** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c6ced-217">When the topology has been submitted, the **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="c6ced-218">Válassza ki, ha a topológiára vonatkozó adatokat a **EventHubReader** topológia a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="c6ced-218">To view information about the topology, select the **EventHubReader** topology in the left pane.</span></span>

    ![A Storm-topológiák megjelenítő képernyőkép](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="c6ced-220">A **Megoldáskezelőben**, kattintson a jobb gombbal a **EventHubWriter** projektre, és válassza a **Submit a HDInsight alatt futó Storm**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-220">From **Solution Explorer**, right-click the **EventHubWriter** project, and select **Submit to Storm on HDInsight**.</span></span>

5. <span data-ttu-id="c6ced-221">Az a **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-221">On the **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="c6ced-222">Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és válassza ki a korábban letöltött JAR-fájlt tartalmazó könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c6ced-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select the directory that contains the JAR file you downloaded earlier.</span></span> <span data-ttu-id="c6ced-223">Végezetül kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="c6ced-224">A topológia elküldésekor frissítse a topológia listáján, a **Storm-topológiák Viewer** annak ellenőrzéséhez, hogy mindkét topológia fut-e a fürtön.</span><span class="sxs-lookup"><span data-stu-id="c6ced-224">When the topology has been submitted, refresh the topology list in the **Storm Topologies Viewer** to verify that both topologies are running on the cluster.</span></span>

7. <span data-ttu-id="c6ced-225">A **Storm-topológiák Viewer**, jelölje be a **EventHubReader** topológia.</span><span class="sxs-lookup"><span data-stu-id="c6ced-225">In **Storm Topologies Viewer**, select the **EventHubReader** topology.</span></span>

8. <span data-ttu-id="c6ced-226">A bolt összegzése összetevő megnyitásához kattintson duplán a **LogBolt** összetevőt a diagramban.</span><span class="sxs-lookup"><span data-stu-id="c6ced-226">To open the component summary for the bolt, double-click the **LogBolt** component in the diagram.</span></span>

9. <span data-ttu-id="c6ced-227">Az a **végrehajtója** területen szereplő hivatkozások közül válassza ki a **Port** oszlop.</span><span class="sxs-lookup"><span data-stu-id="c6ced-227">In the **Executors** section, select one of the links in the **Port** column.</span></span> <span data-ttu-id="c6ced-228">Ez az összetevő által naplózott információk megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="c6ced-228">This displays information logged by the component.</span></span> <span data-ttu-id="c6ced-229">A naplózott információk hasonlít a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="c6ced-229">The logged information is similar to the following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-the-topologies"></a><span data-ttu-id="c6ced-230">A topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="c6ced-230">Stop the topologies</span></span>

<span data-ttu-id="c6ced-231">A topológiák leállításához válassza ki az egyes topológia a **Storm-topológia Viewer**, majd kattintson a **Kill**.</span><span class="sxs-lookup"><span data-stu-id="c6ced-231">To stop the topologies, select each topology in the **Storm Topology Viewer**, then click **Kill**.</span></span>

![Képernyőfelvétel a Storm topológia megjelenítő, a Kill gomb](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="c6ced-233">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="c6ced-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="c6ced-234">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6ced-234">Next steps</span></span>

<span data-ttu-id="c6ced-235">Ebben a dokumentumban rendelkezik megtudta, hogyan használja a Java Event hub spout, és a C#-topológiák adatait az Azure Event Hubs boltok.</span><span class="sxs-lookup"><span data-stu-id="c6ced-235">In this document, you have learned how to use the Java Event Hubs spout and bolt from a C# topology to work with data in Azure Event Hubs.</span></span> <span data-ttu-id="c6ced-236">C#-topológiák létrehozásával kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="c6ced-236">To learn more about creating C# topologies, see the following:</span></span>

* [<span data-ttu-id="c6ced-237">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="c6ced-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="c6ced-238">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="c6ced-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="c6ced-239">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="c6ced-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
