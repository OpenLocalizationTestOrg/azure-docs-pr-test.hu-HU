---
title: "a Storm aaaApache írási tooStorage/Data Lake Store - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello a HDInsight alatt futó Apache Storm toowrite toohello HDFS-kompatibilis tároló. Az Azure Storage vagy az Azure Data Lake Store a HDFS-comptabile hello tárhely biztosításához a HDInsight számára. A dokumentum, és a hello társított példában bemutatják, hogyan hello HdfsBolt összetevő lehet használt toowrite toohello alapértelmezett egy Storm on HDInsight-fürt tárolására."
services: hdinsight
documentationcenter: na
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 1df98653-a6c8-4662-a8c6-5d288fc4f3a6
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/19/2017
ms.author: larryfr
ms.openlocfilehash: d76159a9ecd1be18e519511cfdb3bcfd18ae4d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="write-toohdfs-from-apache-storm-on-hdinsight"></a><span data-ttu-id="0a337-105">A HDInsight alatt futó Apache Storm tooHDFS írása</span><span class="sxs-lookup"><span data-stu-id="0a337-105">Write tooHDFS from Apache Storm on HDInsight</span></span>

<span data-ttu-id="0a337-106">Ismerje meg, hogyan toouse Storm toowrite adatok toohello HDFS-kompatibilis storage a HDInsight alatt futó Apache Storm használja.</span><span class="sxs-lookup"><span data-stu-id="0a337-106">Learn how toouse Storm toowrite data toohello HDFS-compatible storage used by Apache Storm on HDInsight.</span></span> <span data-ttu-id="0a337-107">HDInsight is használhat HDFS-comptabile tárolóként tárolásához Azure Storage és az Azure Data Lake.</span><span class="sxs-lookup"><span data-stu-id="0a337-107">HDInsight can use both Azure Storage and Azure Data Lake store as HDFS-comptabile storage.</span></span> <span data-ttu-id="0a337-108">Storm lehetővé egy [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) írja az adatokat tooHDFS összetevő.</span><span class="sxs-lookup"><span data-stu-id="0a337-108">Storm provides an [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) component that writes data tooHDFS.</span></span> <span data-ttu-id="0a337-109">Ez a dokumentum tájékoztatást nyújt a hello HdfsBolt tooeither a tároló típusa szerinti írása.</span><span class="sxs-lookup"><span data-stu-id="0a337-109">This document provides information on writing tooeither type of storage from hello HdfsBolt.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="0a337-110">hello példában topológia a HDInsight alatt futó Storm részét képező összetevők támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="0a337-110">hello example topology used in this document relies on components that are included with Storm on HDInsight.</span></span> <span data-ttu-id="0a337-111">Megkövetelheti, hogy a módosítás toowork az Azure Data Lake Store más alatt futó Apache Storm-fürtök használata esetén.</span><span class="sxs-lookup"><span data-stu-id="0a337-111">It may require modification toowork with Azure Data Lake Store when used with other Apache Storm clusters.</span></span>

## <a name="get-hello-code"></a><span data-ttu-id="0a337-112">Hello kód beolvasása</span><span class="sxs-lookup"><span data-stu-id="0a337-112">Get hello code</span></span>

<span data-ttu-id="0a337-113">Ez a topológia tartalmazó hello projekt egy letölthető [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span><span class="sxs-lookup"><span data-stu-id="0a337-113">hello project containing this topology is available as a download from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store).</span></span>

<span data-ttu-id="0a337-114">toocompile ebben a projektben van szüksége a fejlesztési környezet konfigurációját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0a337-114">toocompile this project, you need hello following configuration for your development environment:</span></span>

* <span data-ttu-id="0a337-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) vagy újabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="0a337-115">[Java JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) or higher.</span></span> <span data-ttu-id="0a337-116">HDInsight 3.5-ös vagy újabb rendszer szükséges Java 8.</span><span class="sxs-lookup"><span data-stu-id="0a337-116">HDInsight 3.5 or higher require Java 8.</span></span>

* [<span data-ttu-id="0a337-117">Maven 3.x</span><span class="sxs-lookup"><span data-stu-id="0a337-117">Maven 3.x</span></span>](https://maven.apache.org/download.cgi)

<span data-ttu-id="0a337-118">hello következő környezeti változó lehet beállítani a fejlesztő munkaállomás Java és hello JDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="0a337-118">hello following environment variables may be set when you install Java and hello JDK on your development workstation.</span></span> <span data-ttu-id="0a337-119">Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0a337-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="0a337-120">`JAVA_HOME`-toohello rendszert tartalmazó könyvtár hello JDK kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="0a337-120">`JAVA_HOME` - should point toohello directory where hello JDK is installed.</span></span>
* <span data-ttu-id="0a337-121">`PATH`-a következő elérési utak hello tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="0a337-121">`PATH` - should contain hello following paths:</span></span>
  
    * <span data-ttu-id="0a337-122">`JAVA_HOME`(vagy ezzel egyenértékű elérési hello).</span><span class="sxs-lookup"><span data-stu-id="0a337-122">`JAVA_HOME` (or hello equivalent path).</span></span>
    * <span data-ttu-id="0a337-123">`JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello).</span><span class="sxs-lookup"><span data-stu-id="0a337-123">`JAVA_HOME\bin` (or hello equivalent path).</span></span>
    * <span data-ttu-id="0a337-124">hello mappát, ahová a Maven telepítve van.</span><span class="sxs-lookup"><span data-stu-id="0a337-124">hello directory where Maven is installed.</span></span>

## <a name="how-toouse-hello-hdfsbolt-with-hdinsight"></a><span data-ttu-id="0a337-125">Hogyan toouse hello HdfsBolt a hdinsight eszközzel</span><span class="sxs-lookup"><span data-stu-id="0a337-125">How toouse hello HdfsBolt with HDInsight</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a337-126">A HDInsight alatt futó Storm hello HdfsBolt használatához először használnia kell egy parancsfájl művelet szükséges toocopy jar fájlok be hello `extpath` alatt futó Storm.</span><span class="sxs-lookup"><span data-stu-id="0a337-126">Before using hello HdfsBolt with Storm on HDInsight, you must first use a script action toocopy required jar files into hello `extpath` for Storm.</span></span> <span data-ttu-id="0a337-127">További információkért lásd: hello [konfigurálása hello fürt](#configure) szakasz.</span><span class="sxs-lookup"><span data-stu-id="0a337-127">For more information, see hello [Configure hello cluster](#configure) section.</span></span>

<span data-ttu-id="0a337-128">hello HdfsBolt hello fájl sémát, hogy megadja toounderstand hogyan használja toowrite tooHDFS.</span><span class="sxs-lookup"><span data-stu-id="0a337-128">hello HdfsBolt uses hello file scheme that you provide toounderstand how toowrite tooHDFS.</span></span> <span data-ttu-id="0a337-129">A hdinsight eszközzel használja a következő rendszerek hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="0a337-129">With HDInsight, use one of hello following schemes:</span></span>

* <span data-ttu-id="0a337-130">`wasb://`: Egy Azure Storage-fiókot használni.</span><span class="sxs-lookup"><span data-stu-id="0a337-130">`wasb://`: Used with an Azure Storage account.</span></span>
* <span data-ttu-id="0a337-131">`adl://`: Az Azure Data Lake Store használt.</span><span class="sxs-lookup"><span data-stu-id="0a337-131">`adl://`: Used with Azure Data Lake Store.</span></span>

<span data-ttu-id="0a337-132">hello következő táblázat példákat tartalmaz hello fájl séma használatával különböző helyzetek kezelésére.</span><span class="sxs-lookup"><span data-stu-id="0a337-132">hello following table provides examples of using hello file scheme for different scenarios:</span></span>

| <span data-ttu-id="0a337-133">séma</span><span class="sxs-lookup"><span data-stu-id="0a337-133">Scheme</span></span> | <span data-ttu-id="0a337-134">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="0a337-134">Notes</span></span> |
| ----- | ----- |
| `wasb:///` | <span data-ttu-id="0a337-135">hello alapértelmezett tárfiók egy blob tároló, az Azure Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="0a337-135">hello default storage account is a blob container in an Azure Storage account</span></span> |
| `adl:///` | <span data-ttu-id="0a337-136">hello alapértelmezett tárfiók út egy könyvtár az Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0a337-136">hello default storage account is a directory in Azure Data Lake Store.</span></span> <span data-ttu-id="0a337-137">Fürt létrehozása során meg hello directory Data Lake Store, amely hello fürt HDFS hello gyökérmappájában.</span><span class="sxs-lookup"><span data-stu-id="0a337-137">During cluster creation, you specify hello directory in Data Lake Store that is hello root of hello cluster's HDFS.</span></span> <span data-ttu-id="0a337-138">Például hello `/clusters/myclustername/` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="0a337-138">For example, hello `/clusters/myclustername/` directory.</span></span> |
| `wasb://CONTAINER@ACCOUNT.blob.core.windows.net/` | <span data-ttu-id="0a337-139">Egy nem alapértelmezett (további) Azure-tárfiók hello-fürthöz tartozó.</span><span class="sxs-lookup"><span data-stu-id="0a337-139">A non-default (additional) Azure storage account associated with hello cluster.</span></span> |
| `adl://STORENAME/` | <span data-ttu-id="0a337-140">Data Lake Store hello fürt által használt hello hello gyökere.</span><span class="sxs-lookup"><span data-stu-id="0a337-140">hello root of hello Data Lake Store used by hello cluster.</span></span> <span data-ttu-id="0a337-141">A rendszer lehetővé teszi tooaccess adatok hello fürt fájlrendszer tartalmazó hello könyvtár kívül helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="0a337-141">This scheme allows you tooaccess data that is located outside hello directory that contains hello cluster file system.</span></span> |

<span data-ttu-id="0a337-142">További információkért lásd: hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) hivatkozás az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="0a337-142">For more information, see hello [HdfsBolt](http://storm.apache.org/releases/1.1.0/javadocs/org/apache/storm/hdfs/bolt/HdfsBolt.html) reference at Apache.org.</span></span>

### <a name="example-configuration"></a><span data-ttu-id="0a337-143">Példa konfiguráció</span><span class="sxs-lookup"><span data-stu-id="0a337-143">Example configuration</span></span>

<span data-ttu-id="0a337-144">hello következő YAM fájlból hello `resources/writetohdfs.yaml` hello példában szereplő fájlt.</span><span class="sxs-lookup"><span data-stu-id="0a337-144">hello following YAML is an excerpt from hello `resources/writetohdfs.yaml` file included in hello example.</span></span> <span data-ttu-id="0a337-145">Ez a fájl határozza meg a hello Storm-topológia hello segítségével [fluxus](https://storm.apache.org/releases/1.1.0/flux.html) keretrendszere, amely alatt futó Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="0a337-145">This file defines hello Storm topology using hello [Flux](https://storm.apache.org/releases/1.1.0/flux.html) framework for Apache Storm.</span></span>

```yaml
components:
  - id: "syncPolicy"
    className: "org.apache.storm.hdfs.bolt.sync.CountSyncPolicy"
    constructorArgs:
      - 1000

  - id: "rotationPolicy"
    className: "org.apache.storm.hdfs.bolt.rotation.NoRotationPolicy"

  - id: "fileNameFormat"
    className: "org.apache.storm.hdfs.bolt.format.DefaultFileNameFormat"
    configMethods:
      - name: "withPath"
        args: ["${hdfs.write.dir}"]
      - name: "withExtension"
        args: [".txt"]

  - id: "recordFormat"
    className: "org.apache.storm.hdfs.bolt.format.DelimitedRecordFormat"
    configMethods:
      - name: "withFieldDelimiter"
        args: ["|"]

# spout definitions
spouts:
  - id: "tick-spout"
    className: "com.microsoft.example.TickSpout"
    parallelism: 1


# bolt definitions
bolts:
  - id: "hdfs-bolt"
    className: "org.apache.storm.hdfs.bolt.HdfsBolt"
    configMethods:
      - name: "withConfigKey"
        args: ["hdfs.config"]
      - name: "withFsUrl"
        args: ["${hdfs.url}"]
      - name: "withFileNameFormat"
        args: [ref: "fileNameFormat"]
      - name: "withRecordFormat"
        args: [ref: "recordFormat"]
      - name: "withRotationPolicy"
        args: [ref: "rotationPolicy"]
      - name: "withSyncPolicy"
        args: [ref: "syncPolicy"]
```

<span data-ttu-id="0a337-146">A YAM meghatározza, hogy a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0a337-146">This YAML defines hello following items:</span></span>

* <span data-ttu-id="0a337-147">`syncPolicy`: Határozza meg, mikor fájlok legyenek szinkronizálva/kiürített toohello fájlrendszer.</span><span class="sxs-lookup"><span data-stu-id="0a337-147">`syncPolicy`: Defines when files are synched/flushed toohello file system.</span></span> <span data-ttu-id="0a337-148">A példában minden 1000 rekordokat.</span><span class="sxs-lookup"><span data-stu-id="0a337-148">In this example, every 1000 tuples.</span></span>
* <span data-ttu-id="0a337-149">`fileNameFormat`: Hello elérési útját és nevét mintát toouse meghatározza a fájlok írása közben.</span><span class="sxs-lookup"><span data-stu-id="0a337-149">`fileNameFormat`: Defines hello path and file name pattern toouse when writing files.</span></span> <span data-ttu-id="0a337-150">Ebben a példában a szűrő használata futásidőben a hello elérési út van megadva, és hello fájl kiterjesztése `.txt`.</span><span class="sxs-lookup"><span data-stu-id="0a337-150">In this example, hello path is provided at runtime using a filter, and hello file extension is `.txt`.</span></span>
* <span data-ttu-id="0a337-151">`recordFormat`: Az írt hello fájlok hello belső formátuma határozza meg.</span><span class="sxs-lookup"><span data-stu-id="0a337-151">`recordFormat`: Defines hello internal format of hello files written.</span></span> <span data-ttu-id="0a337-152">Ebben a példában szereplő mezők hello határolja `|` karakter.</span><span class="sxs-lookup"><span data-stu-id="0a337-152">In this example, fields are delimited by hello `|` character.</span></span>
* <span data-ttu-id="0a337-153">`rotationPolicy`: Meghatározza, hogy mikor toorotate fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0a337-153">`rotationPolicy`: Defines when toorotate files.</span></span> <span data-ttu-id="0a337-154">Ebben a példában elforgatás nélkül történik.</span><span class="sxs-lookup"><span data-stu-id="0a337-154">In this example, no rotation is performed.</span></span>
* <span data-ttu-id="0a337-155">`hdfs-bolt`: Használja az előző összetevők működését hello hello konfigurációs paraméterei, `HdfsBolt` osztály.</span><span class="sxs-lookup"><span data-stu-id="0a337-155">`hdfs-bolt`: Uses hello previous components as configuration parameters for hello `HdfsBolt` class.</span></span>

<span data-ttu-id="0a337-156">Hello fluxus keretrendszerre további információkért lásd: [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="0a337-156">For more information on hello Flux framework, see [https://storm.apache.org/releases/1.1.0/flux.html](https://storm.apache.org/releases/1.1.0/flux.html).</span></span>

## <a name="configure-hello-cluster"></a><span data-ttu-id="0a337-157">Hello fürt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a337-157">Configure hello cluster</span></span>

<span data-ttu-id="0a337-158">Alapértelmezés szerint a HDInsight alatt futó Storm nem tartalmaz, hogy HdfsBolt használ toocommunicate Azure Storage vagy a Data Lake Store a Storm tartozó classpath hello összetevők.</span><span class="sxs-lookup"><span data-stu-id="0a337-158">By default, Storm on HDInsight does not include hello components that HdfsBolt uses toocommunicate with Azure Storage or Data Lake Store in Storm's classpath.</span></span> <span data-ttu-id="0a337-159">Használjon hello alábbi parancsfájl művelet tooadd ezen összetevők toohello `extlib` könyvtár alatt futó Storm a fürt:</span><span class="sxs-lookup"><span data-stu-id="0a337-159">Use hello following script action tooadd these components toohello `extlib` directory for Storm on your cluster:</span></span>

<span data-ttu-id="0a337-160">| A parancsfájl URI |} Csomópontok tooapply úgy, hogy |} Paraméterek. `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, a felügyelő |} Nincs |}</span><span class="sxs-lookup"><span data-stu-id="0a337-160">| Script URI | Nodes tooapply it to| Parameters | | `https://000aarperiscus.blob.core.windows.net/certs/stormextlib.sh` | Nimbus, Supervisor | None |</span></span>

<span data-ttu-id="0a337-161">Ezt a parancsfájlt a fürt használatáról információkért lásd: hello [Parancsfájlműveletek segítségével testre szabhatja HDInsight-fürtök](./hdinsight-hadoop-customize-cluster-linux.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="0a337-161">For information on using this script with your cluster, see hello [Customize HDInsight clusters using script actions](./hdinsight-hadoop-customize-cluster-linux.md) document.</span></span>

## <a name="build-and-package-hello-topology"></a><span data-ttu-id="0a337-162">Buildelés és a csomag hello topológia</span><span class="sxs-lookup"><span data-stu-id="0a337-162">Build and package hello topology</span></span>

1. <span data-ttu-id="0a337-163">Töltse le a hello példaprojekt [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour fejlesztési környezet.</span><span class="sxs-lookup"><span data-stu-id="0a337-163">Download hello example project from [https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store ](https://github.com/Azure-Samples/hdinsight-storm-azure-data-lake-store) tooyour development environment.</span></span>

2. <span data-ttu-id="0a337-164">Egy parancs parancssori futtatásával Terminálszolgáltatások vagy a rendszerhéj munkamenet módosítása könyvtárak toohello gyökérmappájában hello letöltése.</span><span class="sxs-lookup"><span data-stu-id="0a337-164">From a command prompt, terminal, or shell session, change directories toohello root of hello downloaded project.</span></span> <span data-ttu-id="0a337-165">toobuild és a csomag hello topológia, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a337-165">toobuild and package hello topology, use hello following command:</span></span>
   
        mvn compile package
   
    <span data-ttu-id="0a337-166">Miután hello build és csomagolt befejeződött, nincs-e egy új nevű könyvtár `target`, nevű fájlt tartalmaz, amelyek `StormToHdfs-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="0a337-166">Once hello build and packaging completes, there is a new directory named `target`, that contains a file named `StormToHdfs-1.0-SNAPSHOT.jar`.</span></span> <span data-ttu-id="0a337-167">Ez a fájl fordítása hello topológia tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0a337-167">This file contains hello compiled topology.</span></span>

## <a name="deploy-and-run-hello-topology"></a><span data-ttu-id="0a337-168">Regisztrálhat és futtathat hello topológia</span><span class="sxs-lookup"><span data-stu-id="0a337-168">Deploy and run hello topology</span></span>

1. <span data-ttu-id="0a337-169">A következő parancs toocopy hello topológia toohello HDInsight-fürt hello használata.</span><span class="sxs-lookup"><span data-stu-id="0a337-169">Use hello following command toocopy hello topology toohello HDInsight cluster.</span></span> <span data-ttu-id="0a337-170">Cserélje le **felhasználói** az SSH-felhasználónév hello hello fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="0a337-170">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="0a337-171">Cserélje le **CLUSTERNAME** hello nevű hello fürt.</span><span class="sxs-lookup"><span data-stu-id="0a337-171">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        scp target\StormToHdfs-1.0-SNAPSHOT.jar USER@CLUSTERNAME-ssh.azurehdinsight.net:StormToHdfs1.0-SNAPSHOT.jar
   
    <span data-ttu-id="0a337-172">Amikor a rendszer kéri, adja meg az SSH-felhasználó hello hello fürt létrehozásakor használt hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="0a337-172">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="0a337-173">Ha a jelszó helyett használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="0a337-173">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0a337-174">További információk az `scp` a hdinsight eszközzel, lásd: [az SSH a Hdinsighttal](./hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0a337-174">For more information on using `scp` with HDInsight, see [Use SSH with HDInsight](./hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="0a337-175">Hello feltöltése után használja a következő tooconnect toohello HDInsight-fürtjéhez SSH hello.</span><span class="sxs-lookup"><span data-stu-id="0a337-175">Once hello upload completes, use hello following tooconnect toohello HDInsight cluster using SSH.</span></span> <span data-ttu-id="0a337-176">Cserélje le **felhasználói** az SSH-felhasználónév hello hello fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="0a337-176">Replace **USER** with hello SSH user name you used when creating hello cluster.</span></span> <span data-ttu-id="0a337-177">Cserélje le **CLUSTERNAME** hello nevű hello fürt.</span><span class="sxs-lookup"><span data-stu-id="0a337-177">Replace **CLUSTERNAME** with hello name of hello cluster.</span></span>
   
        ssh USER@CLUSTERNAME-ssh.azurehdinsight.net
   
    <span data-ttu-id="0a337-178">Amikor a rendszer kéri, adja meg az SSH-felhasználó hello hello fürt létrehozásakor használt hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="0a337-178">When prompted, enter hello password used when creating hello SSH user for hello cluster.</span></span> <span data-ttu-id="0a337-179">Ha a jelszó helyett használt nyilvános kulcsot, szükség lehet a toouse hello `-i` paraméter toospecify hello elérési toohello titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="0a337-179">If you used a public key instead of a password, you may need toouse hello `-i` parameter toospecify hello path toohello matching private key.</span></span>
   
   <span data-ttu-id="0a337-180">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="0a337-180">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

3. <span data-ttu-id="0a337-181">A csatlakozás után használata hello következő parancsot a toocreate nevű fájl `dev.properties`:</span><span class="sxs-lookup"><span data-stu-id="0a337-181">Once connected, use hello following command toocreate a file named `dev.properties`:</span></span>

        nano dev.properties

4. <span data-ttu-id="0a337-182">Szöveg hello hello tartalmát, a következő használatát hello `dev.properties` fájlt:</span><span class="sxs-lookup"><span data-stu-id="0a337-182">Use hello following text as hello contents of hello `dev.properties` file:</span></span>

        hdfs.write.dir: /stormdata/
        hdfs.url: wasb:///

    > [!IMPORTANT]
    > <span data-ttu-id="0a337-183">Ez a példa feltételezi, hogy a fürt egy Azure Storage-fiókot használja, mint hello alapértelmezett tároló.</span><span class="sxs-lookup"><span data-stu-id="0a337-183">This example assumes that your cluster uses an Azure Storage account as hello default storage.</span></span> <span data-ttu-id="0a337-184">Ha a fürt használ az Azure Data Lake Store, `hdfs.url: adl:///` helyette.</span><span class="sxs-lookup"><span data-stu-id="0a337-184">If your cluster uses Azure Data Lake Store, use `hdfs.url: adl:///` instead.</span></span>
    
    <span data-ttu-id="0a337-185">toosave hello fájl használata __Ctrl + X__, majd __Y__, és végül __Enter__.</span><span class="sxs-lookup"><span data-stu-id="0a337-185">toosave hello file, use __Ctrl + X__, then __Y__, and finally __Enter__.</span></span> <span data-ttu-id="0a337-186">hello ebben a fájlban értéke hello Data Lake store URL-cím és adatot ír hello könyvtárnevet.</span><span class="sxs-lookup"><span data-stu-id="0a337-186">hello values in this file set hello Data Lake store URL and hello directory name that data is written to.</span></span>

3. <span data-ttu-id="0a337-187">A következő parancs toostart hello topológia hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a337-187">Use hello following command toostart hello topology:</span></span>
   
        storm jar StormToHdfs-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --remote -R /writetohdfs.yaml --filter dev.properties

    <span data-ttu-id="0a337-188">Hello topológia továbbítás toohello Nimbus csomópont hello fürt hello fluxus keretrendszer használatával indítja el.</span><span class="sxs-lookup"><span data-stu-id="0a337-188">This command starts hello topology using hello Flux framework by submitting it toohello Nimbus node of hello cluster.</span></span> <span data-ttu-id="0a337-189">hello topológia definiált hello `writetohdfs.yaml` hello jar fájl.</span><span class="sxs-lookup"><span data-stu-id="0a337-189">hello topology is defined by hello `writetohdfs.yaml` file included in hello jar.</span></span> <span data-ttu-id="0a337-190">Hello `dev.properties` fájl szűrőként átadása, és hello topológia olvassák hello értékek hello fájlban találhatók.</span><span class="sxs-lookup"><span data-stu-id="0a337-190">hello `dev.properties` file is passed as a filter, and hello values contained in hello file are read by hello topology.</span></span>

## <a name="view-output-data"></a><span data-ttu-id="0a337-191">Kimeneti adatok megtekintése</span><span class="sxs-lookup"><span data-stu-id="0a337-191">View output data</span></span>

<span data-ttu-id="0a337-192">tooview hello adatokat, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="0a337-192">tooview hello data, use hello following command:</span></span>

    hdfs dfs -ls /stormdata/

<span data-ttu-id="0a337-193">Ez a topológia által létrehozott hello fájlok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0a337-193">A list of hello files created by this topology is displayed.</span></span>

<span data-ttu-id="0a337-194">hello alábbiakban látható egy példa hello előző parancsok által retuned hello adatok:</span><span class="sxs-lookup"><span data-stu-id="0a337-194">hello following list is an example of hello data retuned by hello previous commands:</span></span>

    Found 30 items
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-0-1488568403092.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-1-1488568404567.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-10-1488568408678.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-11-1488568411636.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-12-1488568411884.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-13-1488568412603.txt
    -rw-r-----+  1 sshuser sshuser       5120 2017-03-03 19:13 /stormdata/hdfs-bolt-3-14-1488568415055.txt

## <a name="stop-hello-topology"></a><span data-ttu-id="0a337-195">Hello topológia leállítása</span><span class="sxs-lookup"><span data-stu-id="0a337-195">Stop hello topology</span></span>

<span data-ttu-id="0a337-196">Storm-topológiák csak a futtatása vagy hello fürtök törlése.</span><span class="sxs-lookup"><span data-stu-id="0a337-196">Storm topologies run until stopped, or hello cluster is deleted.</span></span> <span data-ttu-id="0a337-197">toostop hello topológia, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="0a337-197">toostop hello topology, use hello following command:</span></span>

    storm kill hdfswriter

## <a name="delete-your-cluster"></a><span data-ttu-id="0a337-198">A fürt törlése</span><span class="sxs-lookup"><span data-stu-id="0a337-198">Delete your cluster</span></span>

[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

## <a name="next-steps"></a><span data-ttu-id="0a337-199">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0a337-199">Next steps</span></span>

<span data-ttu-id="0a337-200">Most, hogy megtanulta, hogyan toouse Storm toowrite tooAzure tárolási és az Azure Data Lake Store felderítése más [példák a HDInsight Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="0a337-200">Now that you have learned how toouse Storm toowrite tooAzure Storage and Azure Data Lake Store, discover other [Storm examples for HDInsight](hdinsight-storm-example-topology.md).</span></span>

