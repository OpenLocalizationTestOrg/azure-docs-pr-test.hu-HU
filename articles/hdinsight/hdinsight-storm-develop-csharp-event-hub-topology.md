---
title: "az Event Hubs Storm - Azure HDInsight aaaProcess események |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess adatait az Azure Event Hubs C# Storm-topológia hozza létre a Visual Studio hello segítségével a HDInsight tools Visual Studio."
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
ms.openlocfilehash: 30cd910d80eba066f283197bcbbaf11145bc5524
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-c"></a><span data-ttu-id="0898c-103">Az Azure Event Hubs (C#) futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="0898c-103">Process events from Azure Event Hubs with Storm on HDInsight (C#)</span></span>

<span data-ttu-id="0898c-104">Ismerje meg, hogy az Azure Event Hubs a HDInsight alatt futó Apache Storm toowork.</span><span class="sxs-lookup"><span data-stu-id="0898c-104">Learn how toowork with Azure Event Hubs from Apache Storm on HDInsight.</span></span> <span data-ttu-id="0898c-105">Ez a dokumentum egy C# Storm topológia tooread és írási adatait használja Evbent hubok</span><span class="sxs-lookup"><span data-stu-id="0898c-105">This document uses a C# Storm topology tooread and write data from Evbent Hubs</span></span>

> [!NOTE]
> <span data-ttu-id="0898c-106">Ez a projekt Java verziója: [feldolgozni az eseményeket az Azure Event Hubs (Java) futó Storm](hdinsight-storm-develop-java-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-106">For a Java version of this project, see [Process events from Azure Event Hubs with Storm on HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md).</span></span>

## <a name="scpnet"></a><span data-ttu-id="0898c-107">SCP.NET</span><span class="sxs-lookup"><span data-stu-id="0898c-107">SCP.NET</span></span>

<span data-ttu-id="0898c-108">jelen dokumentumban leírt lépések hello SCP.NET, a NuGet-csomagot, így könnyen toocreate C#-topológiák és összetevők Storm való használathoz a HDInsight használata.</span><span class="sxs-lookup"><span data-stu-id="0898c-108">hello steps in this document use SCP.NET, a NuGet package that makes it easy toocreate C# topologies and components for use with Storm on HDInsight.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0898c-109">Amíg ez a dokumentum lépéseit hello a Visual Studio Windows fejlesztői környezetre támaszkodnak, hello lefordított projekt elküldött tooa Storm on HDInsight-fürt által használt Linux lehet.</span><span class="sxs-lookup"><span data-stu-id="0898c-109">While hello steps in this document rely on a Windows development environment with Visual Studio, hello compiled project can be submitted tooa Storm on HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="0898c-110">Csak a Linux-alapú fürtök létrehozása után 28 2016 októberétől kezdve, SCP.NET topológiákat támogatja.</span><span class="sxs-lookup"><span data-stu-id="0898c-110">Only Linux-based clusters created after October 28, 2016, support SCP.NET topologies.</span></span>

<span data-ttu-id="0898c-111">HDInsight 3.4 és nagyobb használata monó toorun C#-topológiák.</span><span class="sxs-lookup"><span data-stu-id="0898c-111">HDInsight 3.4 and greater use Mono toorun C# topologies.</span></span> <span data-ttu-id="0898c-112">Ez a dokumentum hello példában HDInsight 3.6 működik.</span><span class="sxs-lookup"><span data-stu-id="0898c-112">hello example used in this document works with HDInsight 3.6.</span></span> <span data-ttu-id="0898c-113">Ha a HDInsight a saját .NET megoldások létrehozását tervezi, ellenőrizze a hello [monó kompatibilitási](http://www.mono-project.com/docs/about-mono/compatibility/) lehetséges incompatibilities dokumentumában.</span><span class="sxs-lookup"><span data-stu-id="0898c-113">If you plan on creating your own .NET solutions for HDInsight, check hello [Mono compatibility](http://www.mono-project.com/docs/about-mono/compatibility/) document for potential incompatibilities.</span></span>

### <a name="cluster-versioning"></a><span data-ttu-id="0898c-114">Fürt versioning</span><span class="sxs-lookup"><span data-stu-id="0898c-114">Cluster versioning</span></span>

<span data-ttu-id="0898c-115">hello Microsoft.SCP.Net.SDK NuGet-csomagot a projekthez használt hello főverzióját telepítve a HDInsight alatt futó Storm egyeznie kell.</span><span class="sxs-lookup"><span data-stu-id="0898c-115">hello Microsoft.SCP.Net.SDK NuGet package you use for your project must match hello major version of Storm installed on HDInsight.</span></span> <span data-ttu-id="0898c-116">HDInsight-verziókról 3.5-ös és 3.6 alatt futó Stormot használni 1.x, így ezeken a fürtökön SCP.NET verzió 1.0.x.x kell használnia.</span><span class="sxs-lookup"><span data-stu-id="0898c-116">HDInsight versions 3.5 and 3.6 use Storm 1.x, so you must use SCP.NET version 1.0.x.x with these clusters.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0898c-117">Ebben a dokumentumban hello példa egy HDInsight 3.5-ös vagy 3,6 fürt vár.</span><span class="sxs-lookup"><span data-stu-id="0898c-117">hello example in this document expects an HDInsight 3.5 or 3.6 cluster.</span></span>
>
> <span data-ttu-id="0898c-118">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="0898c-118">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0898c-119">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0898c-119">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="0898c-120">C#-topológiák is kell használnia célként .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="0898c-120">C# topologies must also target .NET 4.5.</span></span>

## <a name="how-toowork-with-event-hubs"></a><span data-ttu-id="0898c-121">Hogyan toowork az Event Hubs</span><span class="sxs-lookup"><span data-stu-id="0898c-121">How toowork with Event Hubs</span></span>

<span data-ttu-id="0898c-122">A Microsoft biztosít a Java-összetevők, amelyek az Event Hubs egy Storm-topológia a használt toocommunicate lehetnek.</span><span class="sxs-lookup"><span data-stu-id="0898c-122">Microsoft provides a set of Java components that can be used toocommunicate with Event Hubs from a Storm topology.</span></span> <span data-ttu-id="0898c-123">Hello Java archív (JAR) fájl, amely tartalmazza ezeket az összetevőket, egy HDInsight 3.6 kompatibilis verzióját [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="0898c-123">You can find hello Java archive (JAR) file that contains an HDInsight 3.6 compatible version of these components at [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0898c-124">Amíg hello összetevők Java nyelven íródtak, könnyen használhatja őket a C#-topológiák.</span><span class="sxs-lookup"><span data-stu-id="0898c-124">While hello components are written in Java, you can easily use them from a C# topology.</span></span>

<span data-ttu-id="0898c-125">a következő összetevők hello szerepelnek ebben a példában:</span><span class="sxs-lookup"><span data-stu-id="0898c-125">hello following components are used in this example:</span></span>

* <span data-ttu-id="0898c-126">__EventHubSpout__: olvassa be az adatokat az Event Hubs.</span><span class="sxs-lookup"><span data-stu-id="0898c-126">__EventHubSpout__: Reads data from Event Hubs.</span></span>
* <span data-ttu-id="0898c-127">__EventHubBolt__: adatok tooEvent hubok írja.</span><span class="sxs-lookup"><span data-stu-id="0898c-127">__EventHubBolt__: Writes data tooEvent Hubs.</span></span>
* <span data-ttu-id="0898c-128">__EventHubSpoutConfig__: tooconfigure EventHubSpout használt.</span><span class="sxs-lookup"><span data-stu-id="0898c-128">__EventHubSpoutConfig__: Used tooconfigure EventHubSpout.</span></span>
* <span data-ttu-id="0898c-129">__EventHubBoltConfig__: tooconfigure EventHubBolt használt.</span><span class="sxs-lookup"><span data-stu-id="0898c-129">__EventHubBoltConfig__: Used tooconfigure EventHubBolt.</span></span>

### <a name="example-spout-usage"></a><span data-ttu-id="0898c-130">Spout-használat – példa</span><span class="sxs-lookup"><span data-stu-id="0898c-130">Example spout usage</span></span>

<span data-ttu-id="0898c-131">SCP.NET egy EventHubSpout tooyour topológia hozzáadására szolgáló módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="0898c-131">SCP.NET provides methods for adding an EventHubSpout tooyour topology.</span></span> <span data-ttu-id="0898c-132">Ezek a módszerek révén könnyebben tooadd egy spout mint hello az általános metódusok használata a Java-összetevő hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="0898c-132">These methods make it easier tooadd a spout than using hello generic methods for adding a Java component.</span></span> <span data-ttu-id="0898c-133">hello következő példa bemutatja, hogyan toocreate használatával egy spout hello __SetEventHubSpout__ és **EventHubSpoutConfig** SCP.NET által biztosított módszerek:</span><span class="sxs-lookup"><span data-stu-id="0898c-133">hello following example demonstrates how toocreate a spout by using hello __SetEventHubSpout__ and **EventHubSpoutConfig** methods provided by SCP.NET:</span></span>

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

<span data-ttu-id="0898c-134">hello előző példa létrehoz egy új spout nevű összetevő __EventHubSpout__, és az eseményközpontban toocommunicate azt konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="0898c-134">hello previous example creates a new spout component named __EventHubSpout__, and configures it toocommunicate with an event hub.</span></span> <span data-ttu-id="0898c-135">hello párhuzamossági mutató hello összetevő hello eseményközpont toohello több partíció van megadva.</span><span class="sxs-lookup"><span data-stu-id="0898c-135">hello parallelism hint for hello component is set toohello number of partitions in hello event hub.</span></span> <span data-ttu-id="0898c-136">Ez a beállítás lehetővé teszi, hogy a Storm toocreate hello minden partíció esetében példányt.</span><span class="sxs-lookup"><span data-stu-id="0898c-136">This setting allows Storm toocreate an instance of hello component for each partition.</span></span>

### <a name="example-bolt-usage"></a><span data-ttu-id="0898c-137">Példa a bolt használatra</span><span class="sxs-lookup"><span data-stu-id="0898c-137">Example bolt usage</span></span>

<span data-ttu-id="0898c-138">Használjon hello **JavaComponmentConstructor** metódus toocreate hello bolt példánya.</span><span class="sxs-lookup"><span data-stu-id="0898c-138">Use hello **JavaComponmentConstructor** method toocreate an instance of hello bolt.</span></span> <span data-ttu-id="0898c-139">hello következő példa bemutatja, hogyan toocreate és állítsa be egy új példányát hello **EventHubBolt**:</span><span class="sxs-lookup"><span data-stu-id="0898c-139">hello following example demonstrates how toocreate and configure a new instance of hello **EventHubBolt**:</span></span>

```csharp
// Java construcvtor for hello Event Hub Bolt
JavaComponentConstructor constructor = JavaComponentConstructor.CreateFromClojureExpr(
    String.Format(@"(org.apache.storm.eventhubs.bolt.EventHubBolt. (org.apache.storm.eventhubs.bolt.EventHubBoltConfig. " +
        @"""{0}"" ""{1}"" ""{2}"" ""{3}"" ""{4}"" {5}))",
        ConfigurationManager.AppSettings["EventHubPolicyName"],
        ConfigurationManager.AppSettings["EventHubPolicyKey"],
        ConfigurationManager.AppSettings["EventHubNamespace"],
        "servicebus.windows.net",
        ConfigurationManager.AppSettings["EventHubName"],
        "true"));

// Set hello bolt toosubscribe toodata from hello spout
topologyBuilder.SetJavaBolt(
    "eventhubbolt",
    constructor,
    partitionCount)
        .shuffleGrouping("Spout");
```

> [!NOTE]
> <span data-ttu-id="0898c-140">Ez a példa egy karakterláncként használata helyett átadott Clojure kifejezés **JavaComponentConstructor** toocreate egy **EventHubBoltConfig**, mint hello spout példa.</span><span class="sxs-lookup"><span data-stu-id="0898c-140">This example uses a Clojure expression passed as a string, instead of using **JavaComponentConstructor** toocreate an **EventHubBoltConfig**, as hello spout example did.</span></span> <span data-ttu-id="0898c-141">Mindkét módszer használható.</span><span class="sxs-lookup"><span data-stu-id="0898c-141">Either method works.</span></span> <span data-ttu-id="0898c-142">Ajánlott tooyou érzi hello módot használ.</span><span class="sxs-lookup"><span data-stu-id="0898c-142">Use hello method that feels best tooyou.</span></span>

## <a name="download-hello-completed-project"></a><span data-ttu-id="0898c-143">Hello befejeződött-projekt letöltése</span><span class="sxs-lookup"><span data-stu-id="0898c-143">Download hello completed project</span></span>

<span data-ttu-id="0898c-144">Ebben az oktatóanyagban a létrehozott hello projekt teljes verziója letölthető [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="0898c-144">You can download a complete version of hello project created in this tutorial from [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span> <span data-ttu-id="0898c-145">Azonban továbbra is szükség tooprovide konfigurációs beállítások ebben az oktatóanyagban hello lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="0898c-145">However, you still need tooprovide configuration settings by following hello steps in this tutorial.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="0898c-146">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0898c-146">Prerequisites</span></span>

* <span data-ttu-id="0898c-147">Egy [Apache Storm on HDInsight-fürt verziószáma 3.5-ös vagy 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-147">An [Apache Storm on HDInsight cluster version 3.5 or 3.6](hdinsight-apache-storm-tutorial-get-started.md).</span></span>

    > [!WARNING]
    > <span data-ttu-id="0898c-148">Ez a dokumentum hello példában 3.5-ös vagy 3.6 HDInsight alatt futó Storm igényel.</span><span class="sxs-lookup"><span data-stu-id="0898c-148">hello example used in this document requires Storm on HDInsight version 3.5 or 3.6.</span></span> <span data-ttu-id="0898c-149">Ez nem működik toobreaking osztály nevének módosítása miatt a HDInsight, korábbi verzióival.</span><span class="sxs-lookup"><span data-stu-id="0898c-149">This does not work with older versions of HDInsight, due toobreaking class name changes.</span></span> <span data-ttu-id="0898c-150">Ebben a példában, amely kompatibilis a régebbi fürtök verziója: [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span><span class="sxs-lookup"><span data-stu-id="0898c-150">For a version of this example that works with older clusters, see [GitHub](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub/releases).</span></span>

* <span data-ttu-id="0898c-151">Egy [Azure event hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-151">An [Azure event hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="0898c-152">Hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="0898c-152">hello [Azure .NET SDK](http://azure.microsoft.com/downloads/).</span></span>

* <span data-ttu-id="0898c-153">Hello [a HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-153">hello [HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

* <span data-ttu-id="0898c-154">Java JDK 1.8 vagy később a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="0898c-154">Java JDK 1.8 or later on your development environment.</span></span> <span data-ttu-id="0898c-155">JDK letöltések érhetők el a [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="0898c-155">JDK downloads are available from [Oracle](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>

  * <span data-ttu-id="0898c-156">Hello **JAVA_HOME** környezeti változó kell pont toohello tartalmazó könyvtár Java.</span><span class="sxs-lookup"><span data-stu-id="0898c-156">hello **JAVA_HOME** environment variable must point toohello directory that contains Java.</span></span>
  * <span data-ttu-id="0898c-157">Hello **%JAVA_HOME%/bin** könyvtárnak hello elérési úton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="0898c-157">hello **%JAVA_HOME%/bin** directory must be in hello path.</span></span>

## <a name="download-hello-event-hubs-components"></a><span data-ttu-id="0898c-158">Hello Event Hubs összetevők letöltése</span><span class="sxs-lookup"><span data-stu-id="0898c-158">Download hello Event Hubs components</span></span>

<span data-ttu-id="0898c-159">Letöltési hello Event Hubs spout, és az összetevőt erről a boltok [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span><span class="sxs-lookup"><span data-stu-id="0898c-159">Download hello Event Hubs spout and bolt component from [https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar](https://github.com/hdinsight/mvn-repo/raw/master/org/apache/storm/storm-eventhubs/1.1.0.1/storm-eventhubs-1.1.0.1.jar).</span></span>

<span data-ttu-id="0898c-160">Hozzon létre egy könyvtárat nevű `eventhubspout`, és mentse a hello fájlt hello könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="0898c-160">Create a directory named `eventhubspout`, and save hello file into hello directory.</span></span>

## <a name="configure-event-hubs"></a><span data-ttu-id="0898c-161">Az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0898c-161">Configure Event Hubs</span></span>

<span data-ttu-id="0898c-162">Az Event Hubs hello adatforrás ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="0898c-162">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="0898c-163">Használja hello hello "Létrehoz egy eseményközpontot" szakaszban található adatokat a [Bevezetés az Event Hubs használatába](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-163">Use hello information in hello "Create an event hub" section of [Get started with Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

1. <span data-ttu-id="0898c-164">Hello eseményközpont létrehozása után megtekintheti a hello **EventHub** panel az Azure portál, majd válassza hello **megosztott elérési házirendek**.</span><span class="sxs-lookup"><span data-stu-id="0898c-164">After hello event hub has been created, view hello **EventHub** blade in hello Azure portal, and select **Shared access policies**.</span></span> <span data-ttu-id="0898c-165">Válassza ki **+ Hozzáadás** tooadd hello következő házirendek:</span><span class="sxs-lookup"><span data-stu-id="0898c-165">Select **+ Add** tooadd hello following policies:</span></span>

   | <span data-ttu-id="0898c-166">Név</span><span class="sxs-lookup"><span data-stu-id="0898c-166">Name</span></span> | <span data-ttu-id="0898c-167">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="0898c-167">Permissions</span></span> |
   | --- | --- |
   | <span data-ttu-id="0898c-168">író</span><span class="sxs-lookup"><span data-stu-id="0898c-168">writer</span></span> |<span data-ttu-id="0898c-169">Küldés</span><span class="sxs-lookup"><span data-stu-id="0898c-169">Send</span></span> |
   | <span data-ttu-id="0898c-170">olvasó</span><span class="sxs-lookup"><span data-stu-id="0898c-170">reader</span></span> |<span data-ttu-id="0898c-171">Figyelés</span><span class="sxs-lookup"><span data-stu-id="0898c-171">Listen</span></span> |

    ![Képernyőkép a fájlmegosztás hozzáférési házirendek ablak](./media/hdinsight-storm-develop-csharp-event-hub-topology/sas.png)

2. <span data-ttu-id="0898c-173">Jelölje be hello **olvasó** és **író** házirendek.</span><span class="sxs-lookup"><span data-stu-id="0898c-173">Select hello **reader** and **writer** policies.</span></span> <span data-ttu-id="0898c-174">Másolja ki és mentse a hello elsődleges kulcs értéke mindkét házirend később használja ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0898c-174">Copy and save hello primary key value for both policies, as these values are used later.</span></span>

## <a name="configure-hello-eventhubwriter"></a><span data-ttu-id="0898c-175">Hello EventHubWriter konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0898c-175">Configure hello EventHubWriter</span></span>

1. <span data-ttu-id="0898c-176">Ha még nem telepítette hello hello HDInsight eszközök legújabb verzióját a Visual Studio, lásd: [első lépései a HDInsight tools for Visual Studio használatával](hdinsight-hadoop-visual-studio-tools-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0898c-176">If you have not already installed hello latest version of hello HDInsight tools for Visual Studio, see [Get started using HDInsight tools for Visual Studio](hdinsight-hadoop-visual-studio-tools-get-started.md).</span></span>

2. <span data-ttu-id="0898c-177">Töltse le a hello megoldást [eventhub-storm-hibrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="0898c-177">Download hello solution from [eventhub-storm-hybrid](https://github.com/Azure-Samples/hdinsight-dotnet-java-storm-eventhub).</span></span>

3. <span data-ttu-id="0898c-178">A hello **EventHubWriter** projektet, nyissa meg hello **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0898c-178">In hello **EventHubWriter** project, open hello **App.config** file.</span></span> <span data-ttu-id="0898c-179">Hello adatait hello eseményközpont korábbi toofill konfigurálta a következő kulcsok hello értékének hello használata:</span><span class="sxs-lookup"><span data-stu-id="0898c-179">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="0898c-180">Kulcs</span><span class="sxs-lookup"><span data-stu-id="0898c-180">Key</span></span> | <span data-ttu-id="0898c-181">Érték</span><span class="sxs-lookup"><span data-stu-id="0898c-181">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="0898c-182">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="0898c-182">EventHubPolicyName</span></span> |<span data-ttu-id="0898c-183">író (Ha használja-e egy másik nevet a hello házirend *küldése* engedéllyel, akkor használja helyette.)</span><span class="sxs-lookup"><span data-stu-id="0898c-183">writer (If you used a different name for hello policy with *Send* permission, use it instead.)</span></span> |
   | <span data-ttu-id="0898c-184">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="0898c-184">EventHubPolicyKey</span></span> |<span data-ttu-id="0898c-185">hello kulcs hello író házirend.</span><span class="sxs-lookup"><span data-stu-id="0898c-185">hello key for hello writer policy.</span></span> |
   | <span data-ttu-id="0898c-186">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="0898c-186">EventHubNamespace</span></span> |<span data-ttu-id="0898c-187">hello névtér, amely tartalmazza az eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="0898c-187">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="0898c-188">EventHubName</span><span class="sxs-lookup"><span data-stu-id="0898c-188">EventHubName</span></span> |<span data-ttu-id="0898c-189">Az eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="0898c-189">Your event hub name.</span></span> |
   | <span data-ttu-id="0898c-190">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="0898c-190">EventHubPartitionCount</span></span> |<span data-ttu-id="0898c-191">a partíciók az eseményközpont hello száma.</span><span class="sxs-lookup"><span data-stu-id="0898c-191">hello number of partitions in your event hub.</span></span> |

4. <span data-ttu-id="0898c-192">Mentse és zárja be a hello **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0898c-192">Save and close hello **App.config** file.</span></span>

## <a name="configure-hello-eventhubreader"></a><span data-ttu-id="0898c-193">Hello EventHubReader konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0898c-193">Configure hello EventHubReader</span></span>

1. <span data-ttu-id="0898c-194">Nyissa meg hello **EventHubReader** projekt.</span><span class="sxs-lookup"><span data-stu-id="0898c-194">Open hello **EventHubReader** project.</span></span>

2. <span data-ttu-id="0898c-195">Nyissa meg hello **App.config** hello fájlt **EventHubReader**.</span><span class="sxs-lookup"><span data-stu-id="0898c-195">Open hello **App.config** file for hello **EventHubReader**.</span></span> <span data-ttu-id="0898c-196">Hello adatait hello eseményközpont korábbi toofill konfigurálta a következő kulcsok hello értékének hello használata:</span><span class="sxs-lookup"><span data-stu-id="0898c-196">Use hello information from hello event hub that you configured earlier toofill in hello value for hello following keys:</span></span>

   | <span data-ttu-id="0898c-197">Kulcs</span><span class="sxs-lookup"><span data-stu-id="0898c-197">Key</span></span> | <span data-ttu-id="0898c-198">Érték</span><span class="sxs-lookup"><span data-stu-id="0898c-198">Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="0898c-199">EventHubPolicyName</span><span class="sxs-lookup"><span data-stu-id="0898c-199">EventHubPolicyName</span></span> |<span data-ttu-id="0898c-200">olvasó (Ha használja-e egy másik nevet a hello házirend *figyelésére* engedéllyel, használja helyette.)</span><span class="sxs-lookup"><span data-stu-id="0898c-200">reader (If you used a different name for hello policy with *listen* permission, use it instead.)</span></span> |
   | <span data-ttu-id="0898c-201">EventHubPolicyKey</span><span class="sxs-lookup"><span data-stu-id="0898c-201">EventHubPolicyKey</span></span> |<span data-ttu-id="0898c-202">hello kulcs hello olvasó házirend.</span><span class="sxs-lookup"><span data-stu-id="0898c-202">hello key for hello reader policy.</span></span> |
   | <span data-ttu-id="0898c-203">EventHubNamespace</span><span class="sxs-lookup"><span data-stu-id="0898c-203">EventHubNamespace</span></span> |<span data-ttu-id="0898c-204">hello névtér, amely tartalmazza az eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="0898c-204">hello namespace that contains your event hub.</span></span> |
   | <span data-ttu-id="0898c-205">EventHubName</span><span class="sxs-lookup"><span data-stu-id="0898c-205">EventHubName</span></span> |<span data-ttu-id="0898c-206">Az eseményközpont neveként.</span><span class="sxs-lookup"><span data-stu-id="0898c-206">Your event hub name.</span></span> |
   | <span data-ttu-id="0898c-207">EventHubPartitionCount</span><span class="sxs-lookup"><span data-stu-id="0898c-207">EventHubPartitionCount</span></span> |<span data-ttu-id="0898c-208">a partíciók az eseményközpont hello száma.</span><span class="sxs-lookup"><span data-stu-id="0898c-208">hello number of partitions in your event hub.</span></span> |

3. <span data-ttu-id="0898c-209">Mentse és zárja be a hello **App.config** fájlt.</span><span class="sxs-lookup"><span data-stu-id="0898c-209">Save and close hello **App.config** file.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="0898c-210">Hello topológiák telepítése</span><span class="sxs-lookup"><span data-stu-id="0898c-210">Deploy hello topologies</span></span>

1. <span data-ttu-id="0898c-211">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **EventHubReader** projektre, és válassza a **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="0898c-211">From **Solution Explorer**, right-click hello **EventHubReader** project, and select **Submit tooStorm on HDInsight**.</span></span>

    ![Képernyőfelvétel a Solution Explorer, a Küldés tooStorm kiemelt hdinsight](./media/hdinsight-storm-develop-csharp-event-hub-topology/submittostorm.png)

2. <span data-ttu-id="0898c-213">A hello **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**.</span><span class="sxs-lookup"><span data-stu-id="0898c-213">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="0898c-214">Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és a korábban letöltött hello JAR-fájlt tartalmazó select hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="0898c-214">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file that you downloaded earlier.</span></span> <span data-ttu-id="0898c-215">Végezetül kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="0898c-215">Finally, click **Submit**.</span></span>

    ![Topológia nyújt képernyőkép párbeszédpanel](./media/hdinsight-storm-develop-csharp-event-hub-topology/submit.png)

3. <span data-ttu-id="0898c-217">Hello topológia elküldésekor hello **Storm-topológiák Viewer** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0898c-217">When hello topology has been submitted, hello **Storm Topologies Viewer** appears.</span></span> <span data-ttu-id="0898c-218">hello topológia, jelölje be hello tooview információ **EventHubReader** topológia hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="0898c-218">tooview information about hello topology, select hello **EventHubReader** topology in hello left pane.</span></span>

    ![A Storm-topológiák megjelenítő képernyőkép](./media/hdinsight-storm-develop-csharp-event-hub-topology/topologyviewer.png)

4. <span data-ttu-id="0898c-220">A **Megoldáskezelőben**, kattintson a jobb gombbal hello **EventHubWriter** projektre, és válassza a **elküldeni a HDInsight tooStorm**.</span><span class="sxs-lookup"><span data-stu-id="0898c-220">From **Solution Explorer**, right-click hello **EventHubWriter** project, and select **Submit tooStorm on HDInsight**.</span></span>

5. <span data-ttu-id="0898c-221">A hello **nyújt topológia** párbeszédpanelen jelölje ki a **Storm-fürt**.</span><span class="sxs-lookup"><span data-stu-id="0898c-221">On hello **Submit Topology** dialog box, select your **Storm Cluster**.</span></span> <span data-ttu-id="0898c-222">Bontsa ki a **további konfigurációs**, jelölje be **Java elérési utat**, jelölje be **...** , és jelölje be hello directory hello JAR-fájlt tartalmazó korábban letöltött.</span><span class="sxs-lookup"><span data-stu-id="0898c-222">Expand **Additional Configurations**, select **Java File Paths**, select **...**, and select hello directory that contains hello JAR file you downloaded earlier.</span></span> <span data-ttu-id="0898c-223">Végezetül kattintson **Submit**.</span><span class="sxs-lookup"><span data-stu-id="0898c-223">Finally, click **Submit**.</span></span>

6. <span data-ttu-id="0898c-224">Hello topológia elküldésekor frissítse hello topológia listáját a hello **Storm-topológiák Viewer** tooverify futtató mindkét topológia hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="0898c-224">When hello topology has been submitted, refresh hello topology list in hello **Storm Topologies Viewer** tooverify that both topologies are running on hello cluster.</span></span>

7. <span data-ttu-id="0898c-225">A **Storm-topológiák Viewer**, jelölje be hello **EventHubReader** topológia.</span><span class="sxs-lookup"><span data-stu-id="0898c-225">In **Storm Topologies Viewer**, select hello **EventHubReader** topology.</span></span>

8. <span data-ttu-id="0898c-226">tooopen hello összetevő összegzése hello bolt, kattintson duplán a hello **LogBolt** hello diagram komponens.</span><span class="sxs-lookup"><span data-stu-id="0898c-226">tooopen hello component summary for hello bolt, double-click hello **LogBolt** component in hello diagram.</span></span>

9. <span data-ttu-id="0898c-227">A hello **végrehajtója** területen válassza ki a hello hivatkozások egyikét a hello **Port** oszlop.</span><span class="sxs-lookup"><span data-stu-id="0898c-227">In hello **Executors** section, select one of hello links in hello **Port** column.</span></span> <span data-ttu-id="0898c-228">Hello összetevő által naplózott adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0898c-228">This displays information logged by hello component.</span></span> <span data-ttu-id="0898c-229">hello naplózott információ a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="0898c-229">hello logged information is similar toohello following text:</span></span>

        2017-03-02 14:51:29.255 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,255 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1830978598,"deviceId":"8566ccbc-034d-45db-883d-d8a31f34068e"}
        2017-03-02 14:51:29.283 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,283 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1756413275,"deviceId":"647a5eff-823d-482f-a8b4-b95b35ae570b"}
        2017-03-02 14:51:29.313 m.s.p.TaskHost [INFO] Received C# STDOUT: 2017-03-02 14:51:29,312 [1] INFO  EventHubReader_LogBolt [(null)] - Received data: {"deviceValue":1108478910,"deviceId":"206a68fa-8264-4d61-9100-bfdb68ee8f0a"}

## <a name="stop-hello-topologies"></a><span data-ttu-id="0898c-230">Állítsa le a hello topológiák</span><span class="sxs-lookup"><span data-stu-id="0898c-230">Stop hello topologies</span></span>

<span data-ttu-id="0898c-231">toostop hello topológiákat, jelölje ki minden egyes topológia hello **Storm-topológia Viewer**, majd kattintson a **Kill**.</span><span class="sxs-lookup"><span data-stu-id="0898c-231">toostop hello topologies, select each topology in hello **Storm Topology Viewer**, then click **Kill**.</span></span>

![Képernyőfelvétel a Storm topológia megjelenítő, a Kill gomb](./media/hdinsight-storm-develop-csharp-event-hub-topology/killtopology.png)

## <a name="delete-your-cluster"></a><span data-ttu-id="0898c-233">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="0898c-233">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="0898c-234">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0898c-234">Next steps</span></span>

<span data-ttu-id="0898c-235">Ebben a dokumentumban megtanulhatta, hogyan toouse hello Java Event Hubs spout és egy C# topológia toowork adatokkal az Azure Event Hubs a boltok.</span><span class="sxs-lookup"><span data-stu-id="0898c-235">In this document, you have learned how toouse hello Java Event Hubs spout and bolt from a C# topology toowork with data in Azure Event Hubs.</span></span> <span data-ttu-id="0898c-236">További információ az létrehozása a C#-topológiák, toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="0898c-236">toolearn more about creating C# topologies, see hello following:</span></span>

* [<span data-ttu-id="0898c-237">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="0898c-237">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)
* [<span data-ttu-id="0898c-238">Szolgáltatáskapcsolódási pont programozási útmutató</span><span class="sxs-lookup"><span data-stu-id="0898c-238">SCP programming guide</span></span>](hdinsight-storm-scp-programming-guide.md)
* [<span data-ttu-id="0898c-239">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="0898c-239">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
