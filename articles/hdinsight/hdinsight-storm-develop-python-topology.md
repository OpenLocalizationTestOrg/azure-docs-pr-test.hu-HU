---
title: "a Python comopnents - Azure HDInsight alatt futó Storm aaaApache |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate az Apache Storm-topológia Python-összetevőket."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
keywords: Apache storm python
ms.assetid: edd0ec4f-664d-4266-910c-6ecc94172ad8
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.openlocfilehash: 143c639623f1992f913900a7c52d6e3f03c701e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-apache-storm-topologies-using-python-on-hdinsight"></a><span data-ttu-id="f34b3-104">Python használata a HDInsight alatt futó Apache Storm-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="f34b3-104">Develop Apache Storm topologies using Python on HDInsight</span></span>

<span data-ttu-id="f34b3-105">Megtudhatja, hogyan toocreate az Apache Storm-topológia Python-összetevőket.</span><span class="sxs-lookup"><span data-stu-id="f34b3-105">Learn how toocreate an Apache Storm topology that uses Python components.</span></span> <span data-ttu-id="f34b3-106">Apache Storm több nyelv használatát, még akkor is lehetővé teszi egy topológia több nyelv toocombine-összetevőt.</span><span class="sxs-lookup"><span data-stu-id="f34b3-106">Apache Storm supports multiple languages, even allowing you toocombine components from several languages in one topology.</span></span> <span data-ttu-id="f34b3-107">hello fluxus keretrendszer (Storm 0.10.0-s bevezetett) lehetővé teszi a tooeasily megoldásokat Python-összetevőket használnak.</span><span class="sxs-lookup"><span data-stu-id="f34b3-107">hello Flux framework (introduced with Storm 0.10.0) allows you tooeasily create solutions that use Python components.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f34b3-108">a dokumentumban szereplő információk hello teszteltük a HDInsight 3.6 alatt futó Storm használatával.</span><span class="sxs-lookup"><span data-stu-id="f34b3-108">hello information in this document was tested using Storm on HDInsight 3.6.</span></span> <span data-ttu-id="f34b3-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="f34b3-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f34b3-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f34b3-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="f34b3-111">Ez a projekt kódját hello érhető el: [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="f34b3-111">hello code for this project is available at [https://github.com/Azure-Samples/hdinsight-python-storm-wordcount](https://github.com/Azure-Samples/hdinsight-python-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f34b3-112">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f34b3-112">Prerequisites</span></span>

* <span data-ttu-id="f34b3-113">Python 2.7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="f34b3-113">Python 2.7 or higher</span></span>

* <span data-ttu-id="f34b3-114">Java JDK 1.8 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="f34b3-114">Java JDK 1.8 or higher</span></span>

* <span data-ttu-id="f34b3-115">3 maven</span><span class="sxs-lookup"><span data-stu-id="f34b3-115">Maven 3</span></span>

* <span data-ttu-id="f34b3-116">(Választható) A helyi Storm környezet.</span><span class="sxs-lookup"><span data-stu-id="f34b3-116">(Optional) A local Storm development environment.</span></span> <span data-ttu-id="f34b3-117">A helyi Storm-környezet csak akkor szükséges, ha azt szeretné, hogy helyileg toorun hello topológia.</span><span class="sxs-lookup"><span data-stu-id="f34b3-117">A local Storm environment is only needed if you want toorun hello topology locally.</span></span> <span data-ttu-id="f34b3-118">További információkért lásd: [a fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span><span class="sxs-lookup"><span data-stu-id="f34b3-118">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html).</span></span>

## <a name="storm-multi-language-support"></a><span data-ttu-id="f34b3-119">A Storm több nyelv támogatása</span><span class="sxs-lookup"><span data-stu-id="f34b3-119">Storm multi-language support</span></span>

<span data-ttu-id="f34b3-120">Apache Storm lett tervezett toowork összetevőkkel bármely programozási nyelv használatával készítettek.</span><span class="sxs-lookup"><span data-stu-id="f34b3-120">Apache Storm was designed toowork with components written using any programming language.</span></span> <span data-ttu-id="f34b3-121">meg kell érteniük a hello összetevői hogyan toowork a hello [Thrift-definíciók alatt futó Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span><span class="sxs-lookup"><span data-stu-id="f34b3-121">hello components must understand how toowork with hello [Thrift definition for Storm](https://github.com/apache/storm/blob/master/storm-core/src/storm.thrift).</span></span> <span data-ttu-id="f34b3-122">Python a modul hello alatt futó Apache Storm projekt, amely lehetővé teszi a Storm tooeasily felület részeként biztosítja.</span><span class="sxs-lookup"><span data-stu-id="f34b3-122">For Python, a module is provided as part of hello Apache Storm project that allows you tooeasily interface with Storm.</span></span> <span data-ttu-id="f34b3-123">A modul található [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span><span class="sxs-lookup"><span data-stu-id="f34b3-123">You can find this module at [https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py](https://github.com/apache/storm/blob/master/storm-multilang/python/src/main/resources/resources/storm.py).</span></span>

<span data-ttu-id="f34b3-124">A Storm egy Java lefutó folyamat, amely hello Java virtuális gép (JVM).</span><span class="sxs-lookup"><span data-stu-id="f34b3-124">Storm is a Java process that runs on hello Java Virtual Machine (JVM).</span></span> <span data-ttu-id="f34b3-125">Az egyéb nyelven írt összetevők vannak végre, mert magában.</span><span class="sxs-lookup"><span data-stu-id="f34b3-125">Components written in other languages are executed as subprocesses.</span></span> <span data-ttu-id="f34b3-126">hello Storm ezek magában stdin/stdout keresztüli üzenetküldés JSON használatával kommunikál.</span><span class="sxs-lookup"><span data-stu-id="f34b3-126">hello Storm communicates with these subprocesses using JSON messages sent over stdin/stdout.</span></span> <span data-ttu-id="f34b3-127">További részleteket a összetevői közötti kommunikáció hello található [Multi-lang protokoll](https://storm.apache.org/documentation/Multilang-protocol.html) dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="f34b3-127">More details on communication between components can be found in hello [Multi-lang Protocol](https://storm.apache.org/documentation/Multilang-protocol.html) documentation.</span></span>

## <a name="python-with-hello-flux-framework"></a><span data-ttu-id="f34b3-128">Python hello fluxus keretrendszerrel</span><span class="sxs-lookup"><span data-stu-id="f34b3-128">Python with hello Flux framework</span></span>

<span data-ttu-id="f34b3-129">hello fluxus keretrendszer lehetővé teszi a Storm-topológiák toodefine elkülönítve hello összetevők.</span><span class="sxs-lookup"><span data-stu-id="f34b3-129">hello Flux framework allows you toodefine Storm topologies separately from hello components.</span></span> <span data-ttu-id="f34b3-130">hello fluxus keretrendszer YAM toodefine hello Storm-topológia használja.</span><span class="sxs-lookup"><span data-stu-id="f34b3-130">hello Flux framework uses YAML toodefine hello Storm topology.</span></span> <span data-ttu-id="f34b3-131">hello következő szövege a következő: példa bemutatja, hogyan tooreference egy Python-összetevő hello YAM dokumentumban:</span><span class="sxs-lookup"><span data-stu-id="f34b3-131">hello following text is an example of how tooreference a Python component in hello YAML document:</span></span>

```yaml
# Spout definitions
spouts:
  - id: "sentence-spout"
    className: "org.apache.storm.flux.wrappers.spouts.FluxShellSpout"
    constructorArgs:
      # Command line
      - ["python", "sentencespout.py"]
      # Output field(s)
      - ["sentence"]
    # parallelism hint
    parallelism: 1
```

<span data-ttu-id="f34b3-132">osztály hello `FluxShellSpout` használt toostart hello van `sentencespout.py` hello spout magában foglaló parancsprogram.</span><span class="sxs-lookup"><span data-stu-id="f34b3-132">hello class `FluxShellSpout` is used toostart hello `sentencespout.py` script that implements hello spout.</span></span>

<span data-ttu-id="f34b3-133">Fluxus hello Python parancsfájlok toobe vár hello `/resources` belül hello jar-fájlra, amely tartalmazza a hello topológia könyvtárába.</span><span class="sxs-lookup"><span data-stu-id="f34b3-133">Flux expects hello Python scripts toobe in hello `/resources` directory inside hello jar file that contains hello topology.</span></span> <span data-ttu-id="f34b3-134">Ezért ez a példa hello Python parancsfájlok tárol hello `/multilang/resources` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="f34b3-134">So this example stores hello Python scripts in hello `/multilang/resources` directory.</span></span> <span data-ttu-id="f34b3-135">Hello `pom.xml` tartalmazza ezt a fájlt a következő XML hello használata:</span><span class="sxs-lookup"><span data-stu-id="f34b3-135">hello `pom.xml` includes this file using hello following XML:</span></span>

```xml
<!-- include hello Python components -->
<resource>
    <directory>${basedir}/multilang</directory>
    <filtering>false</filtering>
</resource>
```

<span data-ttu-id="f34b3-136">A korábban említett van egy `storm.py` fájl, amely megvalósítja az alatt futó Storm hello Thrift-definíciók.</span><span class="sxs-lookup"><span data-stu-id="f34b3-136">As mentioned earlier, there is a `storm.py` file that implements hello Thrift definition for Storm.</span></span> <span data-ttu-id="f34b3-137">hello fluxus tartalmazza `storm.py` automatikusan amikor hello projekt épül, így nem kell tooworry kapcsolatban, beleértve azt.</span><span class="sxs-lookup"><span data-stu-id="f34b3-137">hello Flux framework includes `storm.py` automatically when hello project is built, so you don't have tooworry about including it.</span></span>

## <a name="build-hello-project"></a><span data-ttu-id="f34b3-138">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f34b3-138">Build hello project</span></span>

<span data-ttu-id="f34b3-139">Hello projekt gyökerében hello használja a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f34b3-139">From hello root of hello project, use hello following command:</span></span>

```bash
mvn clean compile package
```

<span data-ttu-id="f34b3-140">Ezzel a paranccsal létrejön egy `target/WordCount-1.0-SNAPSHOT.jar` hello tartalmazó fájl fordítása topológia.</span><span class="sxs-lookup"><span data-stu-id="f34b3-140">This command creates a `target/WordCount-1.0-SNAPSHOT.jar` file that contains hello compiled topology.</span></span>

## <a name="run-hello-topology-locally"></a><span data-ttu-id="f34b3-141">Futtassa helyben a hello topológia</span><span class="sxs-lookup"><span data-stu-id="f34b3-141">Run hello topology locally</span></span>

<span data-ttu-id="f34b3-142">toorun hello topológia helyileg, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="f34b3-142">toorun hello topology locally, use hello following command:</span></span>

```bash
storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -l -R /topology.yaml
```

> [!NOTE]
> <span data-ttu-id="f34b3-143">Ez a parancs a helyi Storm környezet szükséges.</span><span class="sxs-lookup"><span data-stu-id="f34b3-143">This command requires a local Storm development environment.</span></span> <span data-ttu-id="f34b3-144">További információkért lásd: [a fejlesztési környezet létrehozása](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span><span class="sxs-lookup"><span data-stu-id="f34b3-144">For more information, see [Setting up a development environment](http://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html)</span></span>

<span data-ttu-id="f34b3-145">Egyszer hello topológia indul el, akkor bocsát ki adatokat toohello helyi konzol hasonló toohello a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="f34b3-145">Once hello topology starts, it emits information toohello local console similar toohello following text:</span></span>


    24302 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24302 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting the
    24302 [Thread-28] INFO  o.a.s.t.ShellBolt - ShellLog pid:2437, name:counter-bolt Emitting years:160
    24302 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=the, count=599}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=seven, count=302}
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=dwarfs, count=143}
    24303 [Thread-25-sentence-spout-executor[4 4]] INFO  o.a.s.s.ShellSpout - ShellLog pid:2436, name:sentence-spout Emiting hello cow jumped over hello moon
    24303 [Thread-30] INFO  o.a.s.t.ShellBolt - ShellLog pid:2438, name:splitter-bolt Emitting cow
    24303 [Thread-17-log-executor[3 3]] INFO  o.a.s.f.w.b.LogInfoBolt - {word=four, count=160}


<span data-ttu-id="f34b3-146">toostop hello topológia, használjon __Ctrl + C__.</span><span class="sxs-lookup"><span data-stu-id="f34b3-146">toostop hello topology, use __Ctrl + C__.</span></span>

## <a name="run-hello-storm-topology-on-hdinsight"></a><span data-ttu-id="f34b3-147">A HDInsight Storm-topológia hello futtatása</span><span class="sxs-lookup"><span data-stu-id="f34b3-147">Run hello Storm topology on HDInsight</span></span>

1. <span data-ttu-id="f34b3-148">Használjon hello következő parancsot a toocopy hello `WordCount-1.0-SNAPSHOT.jar` tooyour Storm on HDInsight-fürt fájlt:</span><span class="sxs-lookup"><span data-stu-id="f34b3-148">Use hello following command toocopy hello `WordCount-1.0-SNAPSHOT.jar` file tooyour Storm on HDInsight cluster:</span></span>

    ```bash
    scp target\WordCount-1.0-SNAPSHOT.jar sshuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="f34b3-149">Cserélje le `sshuser` hello SSH-felhasználó a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="f34b3-149">Replace `sshuser` with hello SSH user for your cluster.</span></span> <span data-ttu-id="f34b3-150">Cserélje le `mycluster` hello fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="f34b3-150">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="f34b3-151">Előfordulhat, hogy felszólító tooenter hello hello SSH felhasználó jelszavát.</span><span class="sxs-lookup"><span data-stu-id="f34b3-151">You may be prompted tooenter hello password for hello SSH user.</span></span>

    <span data-ttu-id="f34b3-152">További információ az SSH és az SCP használatával, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="f34b3-152">For more information on using SSH and SCP, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="f34b3-153">Hello fájl feltöltése után csatlakoztassa az SSH használatával toohello fürtöt:</span><span class="sxs-lookup"><span data-stu-id="f34b3-153">Once hello file has been uploaded, connect toohello cluster using SSH:</span></span>

    ```bash
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. <span data-ttu-id="f34b3-154">Hello SSH-munkamenetet, a parancs toostart hello topológia hello fürtön a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="f34b3-154">From hello SSH session, use hello following command toostart hello topology on hello cluster:</span></span>

    ```bash
    storm jar WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux -r -R /topology.yaml
    ```

3. <span data-ttu-id="f34b3-155">Hello Storm felhasználói felülete tooview hello topológia hello fürtben is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f34b3-155">You can use hello Storm UI tooview hello topology on hello cluster.</span></span> <span data-ttu-id="f34b3-156">hello Storm felhasználói felülete https://mycluster.azurehdinsight.net/stormui helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="f34b3-156">hello Storm UI is located at https://mycluster.azurehdinsight.net/stormui.</span></span> <span data-ttu-id="f34b3-157">Cserélje le `mycluster` elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="f34b3-157">Replace `mycluster` with your cluster name.</span></span>

> [!NOTE]
> <span data-ttu-id="f34b3-158">Miután elindult, a Storm-topológia fut. csak a</span><span class="sxs-lookup"><span data-stu-id="f34b3-158">Once started, a Storm topology runs until stopped.</span></span> <span data-ttu-id="f34b3-159">toostop hello topológia, hello a következő módszerek bármelyikét használhatja:</span><span class="sxs-lookup"><span data-stu-id="f34b3-159">toostop hello topology, use one of hello following methods:</span></span>
>
> * <span data-ttu-id="f34b3-160">Hello `storm kill TOPOLOGYNAME` hello parancssori parancsot</span><span class="sxs-lookup"><span data-stu-id="f34b3-160">hello `storm kill TOPOLOGYNAME` command from hello command line</span></span>
> * <span data-ttu-id="f34b3-161">Hello **Kill** hello Storm felhasználói felülete gombjára.</span><span class="sxs-lookup"><span data-stu-id="f34b3-161">hello **Kill** button in hello Storm UI.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f34b3-162">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f34b3-162">Next steps</span></span>

<span data-ttu-id="f34b3-163">Tekintse meg a következő dokumentumok más módokon toouse Python és a HDInsight együttes hello:</span><span class="sxs-lookup"><span data-stu-id="f34b3-163">See hello following documents for other ways toouse Python with HDInsight:</span></span>

* [<span data-ttu-id="f34b3-164">Hogyan toouse Python az adatfolyamként történő MapReduce-feladatok</span><span class="sxs-lookup"><span data-stu-id="f34b3-164">How toouse Python for streaming MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)
* [<span data-ttu-id="f34b3-165">Hogyan toouse Python felhasználó definiált függvény (UDF) a Pig és Hive</span><span class="sxs-lookup"><span data-stu-id="f34b3-165">How toouse Python User Defined Functions (UDF) in Pig and Hive</span></span>](hdinsight-python.md)
