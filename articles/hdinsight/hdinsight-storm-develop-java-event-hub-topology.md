---
title: "az Event Hubs Java használatával futó Storm aaaProcess események |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess Event Hubs adatok Java Storm-topológia Maven létre."
services: hdinsight,notification hubs
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 453fa7b0-c8a6-413e-8747-3ac3b71bed86
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/13/2017
ms.author: larryfr
ms.openlocfilehash: 6506f5bc8f6ab0e29350c071a3f84433382038e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="process-events-from-azure-event-hubs-with-storm-on-hdinsight-java"></a><span data-ttu-id="93d9d-103">Az Azure Event Hubs (Java) futó Storm eseményeinek</span><span class="sxs-lookup"><span data-stu-id="93d9d-103">Process events from Azure Event Hubs with Storm on HDInsight (Java)</span></span>

<span data-ttu-id="93d9d-104">Megtudhatja, hogyan toouse Azure Event Hubs a HDInsight alatt futó Storm.</span><span class="sxs-lookup"><span data-stu-id="93d9d-104">Learn how toouse Azure Event Hubs with Storm on HDInsight.</span></span> <span data-ttu-id="93d9d-105">Ebben a példában az Azure Event Hubs Java-alapú összetevők tooread és írási adatait használja.</span><span class="sxs-lookup"><span data-stu-id="93d9d-105">This example uses Java-based components tooread and write data in Azure Event Hubs.</span></span>

<span data-ttu-id="93d9d-106">Az Azure Event Hubs lehetővé teszi nagy mennyiségű tooprocess webhelyek, alkalmazások és eszközök adatait.</span><span class="sxs-lookup"><span data-stu-id="93d9d-106">Azure Event Hubs allows you tooprocess massive amounts of data from websites, apps, and devices.</span></span> <span data-ttu-id="93d9d-107">hello Event Hub spout teszi, hogy könnyen toouse Apache Storm a HDInsight tooanalyze ezek az adatok valós időben.</span><span class="sxs-lookup"><span data-stu-id="93d9d-107">hello Event Hub spout makes it easy toouse Apache Storm on HDInsight tooanalyze this data in real time.</span></span> <span data-ttu-id="93d9d-108">Az Event Hubs boltok hello segítségével a Storm is írhat adatok tooEvent hubok.</span><span class="sxs-lookup"><span data-stu-id="93d9d-108">You can also write data tooEvent Hubs from Storm by using hello Event Hubs bolt.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="93d9d-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="93d9d-109">Prerequisites</span></span>

* <span data-ttu-id="93d9d-110">Egy HDInsight-fürt verziószáma 3.6 Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="93d9d-110">An Apache Storm on HDInsight cluster version 3.6.</span></span> <span data-ttu-id="93d9d-111">További információkért lásd: [Ismerkedés a Storm on HDInsight-fürt](hdinsight-apache-storm-tutorial-get-started-linux.md).</span><span class="sxs-lookup"><span data-stu-id="93d9d-111">For more information, see [Get started with Storm on HDInsight cluster](hdinsight-apache-storm-tutorial-get-started-linux.md).</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="93d9d-112">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="93d9d-112">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="93d9d-113">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="93d9d-113">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="93d9d-114">Egy [Azure Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="93d9d-114">An [Azure Event Hub](../event-hubs/event-hubs-csharp-ephcs-getstarted.md).</span></span>

* <span data-ttu-id="93d9d-115">[Oracle Java fejlesztői készlet (JDK) 8-as verzió](http://www.oracle.com/technetwork/java/javase/downloads/index.html) vagy ezzel egyenértékű, például [OpenJDK](http://openjdk.java.net/).</span><span class="sxs-lookup"><span data-stu-id="93d9d-115">[Oracle Java Developer Kit (JDK) version 8](http://www.oracle.com/technetwork/java/javase/downloads/index.html) or equivalent, such as [OpenJDK](http://openjdk.java.net/).</span></span>

* <span data-ttu-id="93d9d-116">[Maven](https://maven.apache.org/download.cgi): Maven rendszer project build Java-projektek esetében.</span><span class="sxs-lookup"><span data-stu-id="93d9d-116">[Maven](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="93d9d-117">Szöveg- vagy integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="93d9d-117">A text editor or integrated development environment (IDE).</span></span>

    > [!NOTE]
    > <span data-ttu-id="93d9d-118">A szerkesztő vagy az IDE lehet bizonyos funkciók nem ebben a dokumentumban tárgyalt Maven való munkához.</span><span class="sxs-lookup"><span data-stu-id="93d9d-118">Your editor or IDE may have specific functionality for working with Maven that is not addressed in this document.</span></span> <span data-ttu-id="93d9d-119">A szerkesztési környezetben hello képességekre vonatkozó információ: hello hello-termék dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="93d9d-119">For information about hello capabilities of your editing environment, see hello documentation for hello product you are using.</span></span>

    * <span data-ttu-id="93d9d-120">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="93d9d-120">An SSH client.</span></span> <span data-ttu-id="93d9d-121">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="93d9d-121">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="93d9d-122">Hello `ssh` és `scp` parancsok.</span><span class="sxs-lookup"><span data-stu-id="93d9d-122">hello `ssh` and `scp` commands.</span></span> <span data-ttu-id="93d9d-123">Ezek a használt toocopy fájlok toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="93d9d-123">These are used toocopy files toohello HDInsight cluster.</span></span> <span data-ttu-id="93d9d-124">A Windows ezek a Bash Windows 10 rendszeren keresztül kaphat.</span><span class="sxs-lookup"><span data-stu-id="93d9d-124">On Windows, you can get these through Bash on Windows 10.</span></span>

## <a name="understanding-hello-example"></a><span data-ttu-id="93d9d-125">Understanding hello – példa</span><span class="sxs-lookup"><span data-stu-id="93d9d-125">Understanding hello example</span></span>

<span data-ttu-id="93d9d-126">Hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) példa két topológiát tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="93d9d-126">hello [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub) example contains two topologies:</span></span>

<span data-ttu-id="93d9d-127">Hello `resources/writer.yaml` topológia véletlenszerű adatokat tooan Azure Event Hubs ír.</span><span class="sxs-lookup"><span data-stu-id="93d9d-127">hello `resources/writer.yaml` topology writes random data tooan Azure Event Hub.</span></span> <span data-ttu-id="93d9d-128">hello által előállított hello adatokat `DeviceSpout` összetevő, és egy eszköz érték pedig véletlenszerű Eszközazonosító.</span><span class="sxs-lookup"><span data-stu-id="93d9d-128">hello data is generated by hello `DeviceSpout` component, and is a random device ID and device value.</span></span> <span data-ttu-id="93d9d-129">Ezért, van néhány hardver, amely egy karakterlánc-azonosító és a numerikus értéket bocsát ki szimulálva.</span><span class="sxs-lookup"><span data-stu-id="93d9d-129">So it's simulating some hardware that emits a string ID and a numeric value.</span></span>

<span data-ttu-id="93d9d-130">Ezekkel `resources/reader.yaml` topológia beolvassa az adatokat az Event Hubs (hello EventHubWriter, által írt adatok) hello JSON-adatokat elemez és naplózza hello `deviceId` és `deviceValue` adatokat.</span><span class="sxs-lookup"><span data-stu-id="93d9d-130">Thee `resources/reader.yaml` topology reads data from Event Hub (hello data written by EventHubWriter,) parses hello JSON data, and then logs hello `deviceId` and `deviceValue` data.</span></span>

<span data-ttu-id="93d9d-131">hello adatok formázott JSON-dokumentumként előtt tooEvent Hub írása, és ha olvasni hello olvasó elemzi azt JSON kívüli és rekordokat.</span><span class="sxs-lookup"><span data-stu-id="93d9d-131">hello data is formatted as a JSON document before it is written tooEvent Hub, and when read by hello reader it is parsed out of JSON and into tuples.</span></span> <span data-ttu-id="93d9d-132">hello JSON formátum a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="93d9d-132">hello JSON format is as follows:</span></span>

    { "deviceId": "unique identifier", "deviceValue": some value }

### <a name="project-configuration"></a><span data-ttu-id="93d9d-133">Projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="93d9d-133">Project configuration</span></span>

<span data-ttu-id="93d9d-134">Hello `POM.xml` fájl a Maven project konfigurációs információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="93d9d-134">hello `POM.xml` file contains configuration information for this Maven project.</span></span> <span data-ttu-id="93d9d-135">hello érdekes van:</span><span class="sxs-lookup"><span data-stu-id="93d9d-135">hello interesting pieces are:</span></span>

#### <a name="event-hub-components"></a><span data-ttu-id="93d9d-136">Event Hub összetevők</span><span class="sxs-lookup"><span data-stu-id="93d9d-136">Event Hub components</span></span>

<span data-ttu-id="93d9d-137">hello komponenst, amely beolvassa és tooAzure Event Hubs található hello [HDInsight tárház](https://github.com/hdinsight/mvn-rep).</span><span class="sxs-lookup"><span data-stu-id="93d9d-137">hello component that reads and writes tooAzure Event Hubs is located in hello [HDInsight repository](https://github.com/hdinsight/mvn-rep).</span></span> <span data-ttu-id="93d9d-138">következő részeiben arról olvashat hello hello `POM.xml` terhelés hello összetevők fájl ebben a tárházban lévő</span><span class="sxs-lookup"><span data-stu-id="93d9d-138">hello following sections in hello `POM.xml` file load hello components from this repository</span></span>

```xml
<repositories>
    <repository>
        <id>hdinsight-examples</id>
        <url>http://raw.github.com/hdinsight/mvn-repo/master</url>
    </repository>
</repositories>
```

#### <a name="hello-eventhubs-storm-spout-dependency"></a><span data-ttu-id="93d9d-139">hello EventHubs Storm Spout függőség</span><span class="sxs-lookup"><span data-stu-id="93d9d-139">hello EventHubs Storm Spout dependency</span></span>

```xml
<dependency>
    <groupId>com.microsoft</groupId>
    <artifactId>eventhubs</artifactId>
    <version>${storm.eventhub.version}</version>
</dependency>
```

<span data-ttu-id="93d9d-140">Az XML-kód hello eventhubs csomag, amely tartalmaz egy spout az Event Hubs-ből történő olvasáshoz és tooit írásához. a bolt függőséget határoz meg.</span><span class="sxs-lookup"><span data-stu-id="93d9d-140">This xml defines a dependency for hello eventhubs package, which contains both a spout for reading from Event Hubs, and a bolt for writing tooit.</span></span>

```xml
</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

<span data-ttu-id="93d9d-141">Az XML-kód hello projekt toogenerate kimeneti HDInsight 3.5-ös vagy újabb verzióját használja 8, Java konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="93d9d-141">This xml configures hello project toogenerate output for Java 8, which is used by HDInsight 3.5 or higher.</span></span>

#### <a name="hello-maven-shade-plugin"></a><span data-ttu-id="93d9d-142">hello maven-árnyalatát-beépülő modul</span><span class="sxs-lookup"><span data-stu-id="93d9d-142">hello maven-shade-plugin</span></span>

```xml
<!-- build an uber jar -->
<plugin>
<groupId>org.apache.maven.plugins</groupId>
<artifactId>maven-shade-plugin</artifactId>
<version>2.3</version>
<configuration>
    <transformers>
    <!-- Keep us from getting a can't overwrite file error -->
    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer"/>
    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer"/>
    </transformers>
    <!-- Keep us from getting a bad signature error -->
    <filters>
    <filter>
        <artifact>*:*</artifact>
        <excludes>
            <exclude>META-INF/*.SF</exclude>
            <exclude>META-INF/*.DSA</exclude>
            <exclude>META-INF/*.RSA</exclude>
        </excludes>
    </filter>
    </filters>
</configuration>
<executions>
    <execution>
    <phase>package</phase>
    <goals>
        <goal>shade</goal>
    </goals>
    </execution>
</executions>
</plugin>
```

<span data-ttu-id="93d9d-143">Az XML-kód hello megoldás toopackage hello kimeneti be egy uber jar konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="93d9d-143">This xml configures hello solution toopackage hello output into an uber jar.</span></span> <span data-ttu-id="93d9d-144">hello jar hello projekt kód és a szükséges függőségek is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="93d9d-144">hello jar contains both hello project code and required dependencies.</span></span> <span data-ttu-id="93d9d-145">Azt is szolgál:</span><span class="sxs-lookup"><span data-stu-id="93d9d-145">It is also used to:</span></span>

* <span data-ttu-id="93d9d-146">Nevezze át a licencfájlok hello függőségek.</span><span class="sxs-lookup"><span data-stu-id="93d9d-146">Rename license files for hello dependencies.</span></span>
* <span data-ttu-id="93d9d-147">Zárja ki a biztonsági/aláírások.</span><span class="sxs-lookup"><span data-stu-id="93d9d-147">Exclude security/signatures.</span></span>
* <span data-ttu-id="93d9d-148">Győződjön meg arról, hogy többféle megvalósítása hello azonos felület egyesülnek egy bejegyzést.</span><span class="sxs-lookup"><span data-stu-id="93d9d-148">Ensure that multiple implementations of hello same interface are merged into one entry.</span></span>

<span data-ttu-id="93d9d-149">Ezeket a konfigurációs beállításokat futásidőben megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="93d9d-149">These configuration settings prevent errors at runtime.</span></span>

#### <a name="topology-definitions"></a><span data-ttu-id="93d9d-150">Topológia definíciók</span><span class="sxs-lookup"><span data-stu-id="93d9d-150">Topology definitions</span></span>

<span data-ttu-id="93d9d-151">Ez a példa hello [fluxus](https://storm.apache.org/releases/1.1.0/flux.html) keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="93d9d-151">This example uses hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework.</span></span> <span data-ttu-id="93d9d-152">Ezt a keretrendszert YAM toodefine hello topológiák használja.</span><span class="sxs-lookup"><span data-stu-id="93d9d-152">This framework uses YAML toodefine hello topologies.</span></span> <span data-ttu-id="93d9d-153">hello fő előnye, hogy még nem rögzített kódolási hello topológia a Java-kódot.</span><span class="sxs-lookup"><span data-stu-id="93d9d-153">hello primary benefit is that you aren't hard coding hello topology in Java code.</span></span> <span data-ttu-id="93d9d-154">Mivel hello definition YAM, anélkül, hogy toorecompile mindent hello topológia küldés előtt módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="93d9d-154">Since hello definition is YAML, you can change it before submitting hello topology, without having toorecompile everything.</span></span>

<span data-ttu-id="93d9d-155">__Writer.yaml__:</span><span class="sxs-lookup"><span data-stu-id="93d9d-155">__writer.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubwriter"

components:
  # Configure hello Event Hub spout
  - id: "eventhubbolt-config"
    className: "org.apache.storm.eventhubs.bolt.EventHubBoltConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.write.policy.name}"
      - "${eventhub.write.policy.key}"
      - "${eventhub.namespace}"
      - "servicebus.windows.net"
      - "${eventhub.name}"

spouts:
  - id: "device-emulator-spout"
    className: "com.microsoft.example.DeviceSpout"
    parallelism: ${eventhub.partitions}

bolts:
  - id: "eventhub-bolt"
    className: "org.apache.storm.eventhubs.bolt.EventHubBolt"
    constructorArgs:
      - ref: "eventhubbolt-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

# How data flows through hello components
streams:
  - name: "spout -> eventhub" # just a string used for logging
    from: "device-emulator-spout"
    to: "eventhub-bolt"
    grouping:
        type: SHUFFLE

  - name: "spout -> logger"
    from: "device-emulator-spout"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

<span data-ttu-id="93d9d-156">__Reader.yaml__:</span><span class="sxs-lookup"><span data-stu-id="93d9d-156">__reader.yaml__:</span></span>

```yaml
---
# Topology that reads from Event Hubs
name: "eventhubreader"

components:
  # Configure hello Event Hub spout
  - id: "eventhubspout-config"
    className: "org.apache.storm.eventhubs.spout.EventHubSpoutConfig"
    constructorArgs:
      # These are populated from hello .properties file when hello topology is started
      - "${eventhub.read.policy.name}"
      - "${eventhub.read.policy.key}"
      - "${eventhub.namespace}"
      - "${eventhub.name}"
      - ${eventhub.partitions}

spouts:
  - id: "eventhub-spout"
    className: "org.apache.storm.eventhubs.spout.EventHubSpout"
    constructorArgs:
      - ref: "eventhubspout-config" # config declared in components section
    # parallelism hint. This should be hello same as hello number of partitions for your Event Hub, so we read it from hello dev.properties file passed at run time.
    parallelism: ${eventhub.partitions}

bolts:
  # Log information
  - id: "log-bolt"
    className: "org.apache.storm.flux.wrappers.bolts.LogInfoBolt"
    parallelism: 1

  # Parses from JSON into tuples
  - id: "parser-bolt"
    className: "com.microsoft.example.ParserBolt"
    parallelism: ${eventhub.partitions}

# How data flows through hello components
streams:
  - name: "spout -> parser" # just a string used for logging
    from: "eventhub-spout"
    to: "parser-bolt"
    grouping:
        type: SHUFFLE

  - name: "parser -> log-bolt"
    from: "parser-bolt"
    to: "log-bolt"
    grouping:
        type: SHUFFLE
```

#### <a name="tell-hello-topology-about-event-hub"></a><span data-ttu-id="93d9d-157">Adja az Event Hubs hello topológia</span><span class="sxs-lookup"><span data-stu-id="93d9d-157">Tell hello topology about Event Hub</span></span>

<span data-ttu-id="93d9d-158">Futási időben hello `dev.properties` fájl használt toopass hello Eseményközpont toohello topológiája.</span><span class="sxs-lookup"><span data-stu-id="93d9d-158">At run time, hello `dev.properties` file is used toopass hello Event Hub configuration toohello topology.</span></span> <span data-ttu-id="93d9d-159">hello következő példája hello alapértelmezett hello fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="93d9d-159">hello following example is hello default contents of hello file:</span></span>

```yaml
eventhub.write.policy.name: writer
eventhub.write.policy.key: your_key_here
eventhub.read.policy.name: reader
eventhub.read.policy.key: your_key_here
eventhub.namespace: your_namespace_here
eventhub.name: storm
eventhub.partitions: 2
```

## <a name="configure-environment-variables"></a><span data-ttu-id="93d9d-160">Környezeti változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="93d9d-160">Configure environment variables</span></span>

<span data-ttu-id="93d9d-161">hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="93d9d-161">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="93d9d-162">Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="93d9d-162">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="93d9d-163">**JAVA_HOME** -hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="93d9d-163">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="93d9d-164">Például egy Unix vagy Linux terjesztési nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="93d9d-164">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="93d9d-165">A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="93d9d-165">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>
* <span data-ttu-id="93d9d-166">**Elérési út** -elérési utak a következő hello tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="93d9d-166">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="93d9d-167">**JAVA_HOME** (vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="93d9d-167">**JAVA_HOME** (or hello equivalent path)</span></span>
  * <span data-ttu-id="93d9d-168">**JAVA_HOME\bin** (vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="93d9d-168">**JAVA_HOME\bin** (or hello equivalent path)</span></span>
  * <span data-ttu-id="93d9d-169">hello mappát, ahová a Maven telepítve van</span><span class="sxs-lookup"><span data-stu-id="93d9d-169">hello directory where Maven is installed</span></span>

## <a name="configure-event-hub"></a><span data-ttu-id="93d9d-170">Az Event Hubs konfigurálása</span><span class="sxs-lookup"><span data-stu-id="93d9d-170">Configure Event Hub</span></span>

<span data-ttu-id="93d9d-171">Az Event Hubs hello adatforrás ehhez a példához.</span><span class="sxs-lookup"><span data-stu-id="93d9d-171">Event Hubs is hello data source for this example.</span></span> <span data-ttu-id="93d9d-172">A következő lépéseket toocreate egy Eseményközpont hello használata.</span><span class="sxs-lookup"><span data-stu-id="93d9d-172">Use hello following steps toocreate a Event Hub.</span></span>

1. <span data-ttu-id="93d9d-173">A hello [klasszikus Azure portál](https://manage.windowsazure.com), jelölje be **új** > **Service Bus** > **Eseményközpont**  >  **Egyéni létrehozás**.</span><span class="sxs-lookup"><span data-stu-id="93d9d-173">From hello [Azure Classic Portal](https://manage.windowsazure.com), select **NEW** > **Service Bus** > **Event Hub** > **Custom Create**.</span></span>

2. <span data-ttu-id="93d9d-174">A hello **hozzáadása egy új Eseményközpont** képernyőn, adjon meg egy **Eseményközpont neveként**.</span><span class="sxs-lookup"><span data-stu-id="93d9d-174">On hello **Add a new Event Hub** screen, enter an **Event Hub Name**.</span></span> <span data-ttu-id="93d9d-175">Jelölje be hello **régió** toocreate hello csomópontjában, majd hozzon létre egy névteret vagy válasszon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="93d9d-175">Select hello **Region** toocreate hello hub in, and then create a namespace or select an existing one.</span></span> <span data-ttu-id="93d9d-176">Végül kattintson a hello **nyíl** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="93d9d-176">Finally, click hello **Arrow** toocontinue.</span></span>

    ![1. varázslólap](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz1.png)

   > [!NOTE]
   > <span data-ttu-id="93d9d-178">Válassza ki az azonos hello **hely** , a Storm on HDInsight server tooreduce késleltetés és a költségeket.</span><span class="sxs-lookup"><span data-stu-id="93d9d-178">Select hello same **Location** as your Storm on HDInsight server tooreduce latency and costs.</span></span>

3. <span data-ttu-id="93d9d-179">A hello **konfigurálása az Event Hubs** képernyőn, adja meg a hello **száma partícióazonosító** és **üzenet megőrzési** értékek.</span><span class="sxs-lookup"><span data-stu-id="93d9d-179">On hello **Configure Event Hub** screen, enter hello **Partition count** and **Message Retention** values.</span></span> <span data-ttu-id="93d9d-180">Az ebben a példában a partíciók száma 10 és a egy üzenet megőrzési 1 használhat.</span><span class="sxs-lookup"><span data-stu-id="93d9d-180">For this example, use a partition count of 10 and a message retention of 1.</span></span> <span data-ttu-id="93d9d-181">Megjegyzés: hello partíciók száma, mert később szüksége ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="93d9d-181">Note hello partition count because you need this value later.</span></span>

    ![2. varázslólap](./media/hdinsight-storm-develop-csharp-event-hub-topology/wiz2.png)

4. <span data-ttu-id="93d9d-183">Hello eseményközpont létre, jelölje be hello névtér után válassza ki a **Event Hubs**, majd válassza ki a korábban létrehozott eseményközpont hello.</span><span class="sxs-lookup"><span data-stu-id="93d9d-183">After hello event hub has been created, select hello namespace, select **Event Hubs**, and then select hello event hub that you created earlier.</span></span>
5. <span data-ttu-id="93d9d-184">Válassza ki **konfigurálása**, majd hozzon létre két új hozzáférési házirendek segítségével a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="93d9d-184">Select **Configure**, then create two new access policies by using hello following information:</span></span>

    <table>
    <tr><th><span data-ttu-id="93d9d-185">Név</span><span class="sxs-lookup"><span data-stu-id="93d9d-185">Name</span></span></th><th><span data-ttu-id="93d9d-186">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="93d9d-186">Permissions</span></span></th></tr>
    <tr><td><span data-ttu-id="93d9d-187">író</span><span class="sxs-lookup"><span data-stu-id="93d9d-187">Writer</span></span></td><td><span data-ttu-id="93d9d-188">Küldés</span><span class="sxs-lookup"><span data-stu-id="93d9d-188">Send</span></span></td></tr>
    <tr><td><span data-ttu-id="93d9d-189">Olvasó</span><span class="sxs-lookup"><span data-stu-id="93d9d-189">Reader</span></span></td><td><span data-ttu-id="93d9d-190">Figyelés</span><span class="sxs-lookup"><span data-stu-id="93d9d-190">Listen</span></span></td></tr>
    </table>

    <span data-ttu-id="93d9d-191">Miután létrehozta a hello engedélyek, válassza ki a hello **mentése** alján hello hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="93d9d-191">After You create hello permissions, select hello **Save** icon at hello bottom of hello page.</span></span> <span data-ttu-id="93d9d-192">A megosztott elérési házirendek használt tooread és tooEvent Hub írni.</span><span class="sxs-lookup"><span data-stu-id="93d9d-192">These shared access policies are used tooread and write tooEvent Hub.</span></span>

    ![házirendek](./media/hdinsight-storm-develop-csharp-event-hub-topology/policy.png)

6. <span data-ttu-id="93d9d-194">Miután menti hello házirendek, a hello **megosztott megosztottelérésikulcs-készítő** hello lap tooretrieve hello kulcsa hello hello alján **író** és **olvasó** házirendek.</span><span class="sxs-lookup"><span data-stu-id="93d9d-194">After you save hello policies, use hello **Shared access key generator** at hello bottom of hello page tooretrieve hello key for hello **writer** and **reader** policies.</span></span> <span data-ttu-id="93d9d-195">Ezek a kulcsok mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="93d9d-195">Save these keys.</span></span>

## <a name="download-and-build-hello-project"></a><span data-ttu-id="93d9d-196">Töltse le és hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="93d9d-196">Download and build hello project</span></span>

1. <span data-ttu-id="93d9d-197">Töltse le a hello projektet a Githubról: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span><span class="sxs-lookup"><span data-stu-id="93d9d-197">Download hello project from GitHub: [hdinsight-java-storm-eventhub](https://github.com/Azure-Samples/hdinsight-java-storm-eventhub).</span></span> <span data-ttu-id="93d9d-198">Hello csomag, mint a zip-archívum létrehozása, vagy [git](https://git-scm.com/) tooclone hello projekt helyi küldése.</span><span class="sxs-lookup"><span data-stu-id="93d9d-198">You can either download hello package as a zip archive, or use [git](https://git-scm.com/) tooclone hello project locally.</span></span>

2. <span data-ttu-id="93d9d-199">Módosítsa a hello `dev.properties` az Eseményközpont hello konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="93d9d-199">Modify hello `dev.properties` file with hello configuration for your Event Hub.</span></span>

3. <span data-ttu-id="93d9d-200">A következő toobuild és a csomag hello projekt hello használata:</span><span class="sxs-lookup"><span data-stu-id="93d9d-200">Use hello following toobuild and package hello project:</span></span>

        mvn package

    <span data-ttu-id="93d9d-201">Ez a parancs letölti a szükséges függőségek, buildek, és majd csomagok hello projekt.</span><span class="sxs-lookup"><span data-stu-id="93d9d-201">This command downloads required dependencies, builds, and then packages hello project.</span></span> <span data-ttu-id="93d9d-202">hello kimeneti tárolódik hello **/target** könyvtárhoz, mint a **EventHubExample-1.0-SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="93d9d-202">hello output is stored in hello **/target** directory as **EventHubExample-1.0-SNAPSHOT.jar**.</span></span>

## <a name="test-locally"></a><span data-ttu-id="93d9d-203">Helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="93d9d-203">Test locally</span></span>

<span data-ttu-id="93d9d-204">Mivel a topológiák csak olvasási és írási tooEvent hubok, tesztelheti őket helyileg, ha rendelkezik egy [Storm fejlesztőkörnyezet](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="93d9d-204">Since these topologies just read and write tooEvent Hubs, you can test them locally if you have a [Storm development environment](http://storm.apache.org/releases/current/Setting-up-development-environment.html).</span></span> <span data-ttu-id="93d9d-205">A következő lépéseket toorun helyi környezetben hello fejlesztői hello használata:</span><span class="sxs-lookup"><span data-stu-id="93d9d-205">Use hello following steps toorun locally in hello dev environment:</span></span>

1. <span data-ttu-id="93d9d-206">Futtassa a hello író:</span><span class="sxs-lookup"><span data-stu-id="93d9d-206">Run hello writer:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /writer.yaml --filter dev.properties

2. <span data-ttu-id="93d9d-207">Hello olvasó futtassa:</span><span class="sxs-lookup"><span data-stu-id="93d9d-207">Run hello reader:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /reader.yaml --filter dev.properties

> [!TIP]
> * <span data-ttu-id="93d9d-208">`--local`: Hello topológia helyi módban fusson (nem osztott).</span><span class="sxs-lookup"><span data-stu-id="93d9d-208">`--local`: Run hello topology in local mode (non-distributed).</span></span>
> * <span data-ttu-id="93d9d-209">`-R /writer.yaml`: Hello topológia definíciójának betöltése hello `resources` hello jar csomagolva.</span><span class="sxs-lookup"><span data-stu-id="93d9d-209">`-R /writer.yaml`: Load hello topology definition from hello `resources` packaged in hello jar.</span></span> <span data-ttu-id="93d9d-210">Ha hello topológia egy fájl hello helyi fájlrendszerben, ehelyett adja meg hello elérési tooit hello utolsó paraméterként.</span><span class="sxs-lookup"><span data-stu-id="93d9d-210">If hello topology is a file on hello local file system, specify hello path tooit as hello last parameter instead.</span></span>
> * <span data-ttu-id="93d9d-211">`--filter dev.properties`: Hello tartalmát használja `dev.properties` toofill hello topológia definíciókban hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="93d9d-211">`--filter dev.properties`: Use hello contents of `dev.properties` toofill in hello values in hello topology definitions.</span></span> <span data-ttu-id="93d9d-212">Például: `${eventhub.read.policy.name}`.</span><span class="sxs-lookup"><span data-stu-id="93d9d-212">For example, `${eventhub.read.policy.name}`.</span></span>

<span data-ttu-id="93d9d-213">A helyi futtatás során eredménye a naplózott toohello konzol.</span><span class="sxs-lookup"><span data-stu-id="93d9d-213">Output is logged toohello console when running locally.</span></span> <span data-ttu-id="93d9d-214">Használjon __Ctrl + C__ toostop hello topológia.</span><span class="sxs-lookup"><span data-stu-id="93d9d-214">Use __Ctrl+C__ toostop hello topology.</span></span>

## <a name="deploy-hello-topologies"></a><span data-ttu-id="93d9d-215">Hello topológiák telepítése</span><span class="sxs-lookup"><span data-stu-id="93d9d-215">Deploy hello topologies</span></span>

1. <span data-ttu-id="93d9d-216">Szolgáltatáskapcsolódási pont toocopy hello jar csomag tooyour HDInsight-fürt használatára.</span><span class="sxs-lookup"><span data-stu-id="93d9d-216">Use SCP toocopy hello jar package tooyour HDInsight cluster.</span></span> <span data-ttu-id="93d9d-217">Cserélje le felhasználónév hello SSH-felhasználó a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="93d9d-217">Replace USERNAME with hello SSH user for your cluster.</span></span> <span data-ttu-id="93d9d-218">CLUSTERNAME cserélje le a HDInsight fürt hello neve:</span><span class="sxs-lookup"><span data-stu-id="93d9d-218">Replace CLUSTERNAME with hello name of your HDInsight cluster:</span></span>

        scp ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.

    <span data-ttu-id="93d9d-219">Ha SSH-fiókját használja jelszó, felszólító tooenter hello jelszót is.</span><span class="sxs-lookup"><span data-stu-id="93d9d-219">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="93d9d-220">Ha SSH-kulcsot használt hello fiókkal, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello kulcsfájlt.</span><span class="sxs-lookup"><span data-stu-id="93d9d-220">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="93d9d-221">Például: `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span><span class="sxs-lookup"><span data-stu-id="93d9d-221">For example, `scp -i ~/.ssh/id_rsa ./target/EventHubExample-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:.`</span></span>

    <span data-ttu-id="93d9d-222">Ez a parancs átmásolja hello fájl toohello a SSH felhasználójának kezdőkönyvtárát hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="93d9d-222">This command copies hello file toohello home directory of your SSH user on hello cluster.</span></span>

2. <span data-ttu-id="93d9d-223">Miután hello fájl befejezte a feltöltést, használja az SSH tooconnect toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="93d9d-223">Once hello file has finished uploading, use SSH tooconnect toohello HDInsight cluster.</span></span> <span data-ttu-id="93d9d-224">Cserélje le **felhasználónév** az SSH-bejelentkezéskor hello nevét.</span><span class="sxs-lookup"><span data-stu-id="93d9d-224">Replace **USERNAME** hello name of your SSH login.</span></span> <span data-ttu-id="93d9d-225">Cserélje le **CLUSTERNAME** a HDInsight fürt nevű:</span><span class="sxs-lookup"><span data-stu-id="93d9d-225">Replace **CLUSTERNAME** with your HDInsight cluster name:</span></span>

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    > [!NOTE]
    > <span data-ttu-id="93d9d-226">Ha SSH-fiókját használja jelszó, felszólító tooenter hello jelszót is.</span><span class="sxs-lookup"><span data-stu-id="93d9d-226">If you used a password for your SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="93d9d-227">Ha SSH-kulcsot használt hello fiókkal, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello kulcsfájlt.</span><span class="sxs-lookup"><span data-stu-id="93d9d-227">If you used an SSH key with hello account, you may need toouse hello `-i` parameter toospecify hello path toohello key file.</span></span> <span data-ttu-id="93d9d-228">hello alábbi példa betölti hello titkos kulcsát a `~/.ssh/id_rsa`:</span><span class="sxs-lookup"><span data-stu-id="93d9d-228">hello following example loads hello private key from `~/.ssh/id_rsa`:</span></span>
    >
    > `ssh -i ~/.ssh/id_rsa USERNAME@CLUSTERNAME-ssh.azurehdinsight.net`

3. <span data-ttu-id="93d9d-229">A következő parancs toostart hello topológiák hello használata:</span><span class="sxs-lookup"><span data-stu-id="93d9d-229">Use hello following command toostart hello topologies:</span></span>

        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writer.yaml --filter dev.properties
        storm jar EventHubExample-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /reader.yaml --filter dev.properties

    > [!TIP]
    > * <span data-ttu-id="93d9d-230">`--remote`: Hello topológia toohello Nimbus szolgáltatást, amely elindítja hello munkavégző csomópontokhoz hello fürt küldi el.</span><span class="sxs-lookup"><span data-stu-id="93d9d-230">`--remote`: Submits hello topology toohello Nimbus service, which starts it on hello worker nodes in hello cluster.</span></span>

4. <span data-ttu-id="93d9d-231">tooview hello naplózott adatok, nyissa meg toohttps://CLUSTERNAME.azurehdinsight.net/stormui, ahol __CLUSTERNAME__ hello neve, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="93d9d-231">tooview hello logged data, go toohttps://CLUSTERNAME.azurehdinsight.net/stormui, where __CLUSTERNAME__ is hello name of your HDInsight cluster.</span></span> <span data-ttu-id="93d9d-232">Válassza ki a hello topológiákat, és részletekbe menően tárhatják toohello összetevőket.</span><span class="sxs-lookup"><span data-stu-id="93d9d-232">Select hello topologies and drill down toohello components.</span></span> <span data-ttu-id="93d9d-233">Jelölje be hello __port__ egy összetevő tooview példányának bejegyzés információt naplózta.</span><span class="sxs-lookup"><span data-stu-id="93d9d-233">Select hello __port__ entry for an instance of a component tooview logged information.</span></span>

5. <span data-ttu-id="93d9d-234">A következő parancsok toostop hello topológiák hello használata:</span><span class="sxs-lookup"><span data-stu-id="93d9d-234">Use hello following commands toostop hello topologies:</span></span>

        storm kill reader
        storm kill writer

## <a name="delete-your-cluster"></a><span data-ttu-id="93d9d-235">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="93d9d-235">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="93d9d-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="93d9d-236">Next steps</span></span>

* [<span data-ttu-id="93d9d-237">HDInsight alatt futó Storm példatopológiái</span><span class="sxs-lookup"><span data-stu-id="93d9d-237">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)
