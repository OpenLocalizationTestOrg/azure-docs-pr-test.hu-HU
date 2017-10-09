---
title: a HDInsight hadooppal aaaMapReduce |} Microsoft Docs
description: "Ismerje meg, hogyan toorun MapReduce feladatok hadoop HDInsight-fürtök."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f321501-d62c-4ffc-b5d6-102ecba6dd76
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0cf7ad0e6769e678be64f9e4ec8ed7a214ab7af2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a><span data-ttu-id="10629-103">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="10629-103">Use MapReduce in Hadoop on HDInsight</span></span>

<span data-ttu-id="10629-104">Ismerje meg, hogyan toorun MapReduce feladatok a HDInsight-fürtökön.</span><span class="sxs-lookup"><span data-stu-id="10629-104">Learn how toorun MapReduce jobs on HDInsight clusters.</span></span> <span data-ttu-id="10629-105">A következő tábla toodiscover hello módszert arról, hogy használható-e a MapReduce with HDInsight hello használata:</span><span class="sxs-lookup"><span data-stu-id="10629-105">Use hello following table toodiscover hello various ways that MapReduce can be used with HDInsight:</span></span>

| <span data-ttu-id="10629-106">**Ezzel**...</span><span class="sxs-lookup"><span data-stu-id="10629-106">**Use this**...</span></span> | <span data-ttu-id="10629-107">**.. .toodo Ez**</span><span class="sxs-lookup"><span data-stu-id="10629-107">**...toodo this**</span></span> | <span data-ttu-id="10629-108">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="10629-108">...with this **cluster operating system**</span></span> | <span data-ttu-id="10629-109">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="10629-109">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="10629-110">SSH</span><span class="sxs-lookup"><span data-stu-id="10629-110">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="10629-111">Hello Hadoop paranccsal keresztül **SSH**</span><span class="sxs-lookup"><span data-stu-id="10629-111">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="10629-112">Linux</span><span class="sxs-lookup"><span data-stu-id="10629-112">Linux</span></span> |<span data-ttu-id="10629-113">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-113">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="10629-114">REST</span><span class="sxs-lookup"><span data-stu-id="10629-114">REST</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="10629-115">Hello feladat elküldése távolról használatával **REST** (példák használata cURL)</span><span class="sxs-lookup"><span data-stu-id="10629-115">Submit hello job remotely by using **REST** (examples use cURL)</span></span> |<span data-ttu-id="10629-116">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-116">Linux or Windows</span></span> |<span data-ttu-id="10629-117">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="10629-118">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="10629-118">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="10629-119">Hello feladat elküldése távolról használatával **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10629-119">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="10629-120">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-120">Linux or Windows</span></span> |<span data-ttu-id="10629-121">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-121">Windows</span></span> |
| <span data-ttu-id="10629-122">[A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="10629-122">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="10629-123">Hello Hadoop paranccsal keresztül **távoli asztal**</span><span class="sxs-lookup"><span data-stu-id="10629-123">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="10629-124">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-124">Windows</span></span> |<span data-ttu-id="10629-125">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-125">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="10629-126">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="10629-126">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="10629-127">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="10629-127">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="10629-128"><a id="whatis"></a>Mi az a MapReduce</span><span class="sxs-lookup"><span data-stu-id="10629-128"><a id="whatis"></a>What is MapReduce</span></span>

<span data-ttu-id="10629-129">Hadoop-MapReduce egy szoftveres keretrendszer feladatok, amelyek feldolgozzák a nagy mennyiségű adat írásához.</span><span class="sxs-lookup"><span data-stu-id="10629-129">Hadoop MapReduce is a software framework for writing jobs that process vast amounts of data.</span></span> <span data-ttu-id="10629-130">A bemeneti adatok független adattömböket, majd párhuzamosan futnak hello a fürtben található csomópontok között oszlik meg.</span><span class="sxs-lookup"><span data-stu-id="10629-130">Input data is split into independent chunks, which are then processed in parallel across hello nodes in your cluster.</span></span> <span data-ttu-id="10629-131">A MapReduce feladatot két funkciókat foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="10629-131">A MapReduce job consists of two functions:</span></span>

* <span data-ttu-id="10629-132">**Eseményleképező**: bemeneti adatforrástól, elemzi azokat (általában a szűrési és rendezési műveletek) és bocsát ki a listának (kulcs-érték párok)</span><span class="sxs-lookup"><span data-stu-id="10629-132">**Mapper**: Consumes input data, analyzes it (usually with filter and sorting operations), and emits tuples (key-value pairs)</span></span>

* <span data-ttu-id="10629-133">**Nyomáscsökkentő**: hello leképező által kibocsátott rekordokat használ fel, és hozza létre a kisebb, kombinált eredményeként a hello leképező adatok összegzési műveletet hajt végre</span><span class="sxs-lookup"><span data-stu-id="10629-133">**Reducer**: Consumes tuples emitted by hello Mapper and performs a summary operation that creates a smaller, combined result from hello Mapper data</span></span>

<span data-ttu-id="10629-134">Egy alapszintű word száma MapReduce feladatot példa mutatja be a következő diagram hello:</span><span class="sxs-lookup"><span data-stu-id="10629-134">A basic word count MapReduce job example is illustrated in hello following diagram:</span></span>

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

<span data-ttu-id="10629-136">hello a feladat eredménye hányszor hello szöveg, amelynek történt meg minden szó történt számát.</span><span class="sxs-lookup"><span data-stu-id="10629-136">hello output of this job is a count of how many times each word occurred in hello text that was analyzed.</span></span>

* <span data-ttu-id="10629-137">hello leképező minden sor a bemeneti szöveg hello bemenetként vesz igénybe, és elindítja azt szavakat.</span><span class="sxs-lookup"><span data-stu-id="10629-137">hello mapper takes each line from hello input text as an input and breaks it into words.</span></span> <span data-ttu-id="10629-138">Az megfelelően kibocsát egy kulcs/érték pár minden alkalommal, amikor egy word akkor fordul elő, a hello szó a egy 1 követ.</span><span class="sxs-lookup"><span data-stu-id="10629-138">It emits a key/value pair each time a word occurs of hello word is followed by a 1.</span></span> <span data-ttu-id="10629-139">hello kimeneti tooreducer elküldés előtt van rendezve.</span><span class="sxs-lookup"><span data-stu-id="10629-139">hello output is sorted before sending it tooreducer.</span></span>
* <span data-ttu-id="10629-140">hello nyomáscsökkentő ezeket a minden egyes szó egyedi számokat összegzi, és megfelelően kibocsát egy egyetlen kulcs/érték pár hello szó, és az előfordulások hello összegét tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="10629-140">hello reducer sums these individual counts for each word and emits a single key/value pair that contains hello word followed by hello sum of its occurrences.</span></span>

<span data-ttu-id="10629-141">MapReduce különböző nyelveken valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="10629-141">MapReduce can be implemented in various languages.</span></span> <span data-ttu-id="10629-142">Java hello legelterjedtebb, és a jelen dokumentum bemutatási célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="10629-142">Java is hello most common implementation, and is used for demonstration purposes in this document.</span></span>

## <a name="development-languages"></a><span data-ttu-id="10629-143">Fejlesztési nyelveket</span><span class="sxs-lookup"><span data-stu-id="10629-143">Development languages</span></span>

<span data-ttu-id="10629-144">Nyelvet vagy keretrendszert, amely Java alapulnak, és a Java virtuális gép hello közvetlenül, a MapReduce feladatot is futott.</span><span class="sxs-lookup"><span data-stu-id="10629-144">Languages or frameworks that are based on Java and hello Java Virtual Machine can be ran directly as a MapReduce job.</span></span> <span data-ttu-id="10629-145">hello itt példája egy Java-MapReduce-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10629-145">hello example used in this document is a Java MapReduce application.</span></span> <span data-ttu-id="10629-146">Nem-Java nyelven, például a C#, Python vagy önálló végrehajtható fájlok, a Hadoop streamelési kell használnia.</span><span class="sxs-lookup"><span data-stu-id="10629-146">Non-Java languages, such as C#, Python, or standalone executables, must use Hadoop streaming.</span></span>

<span data-ttu-id="10629-147">Hadoop streamelési keresztül kommunikál a hello leképező és nyomáscsökkentő STDIN és a STDOUT.</span><span class="sxs-lookup"><span data-stu-id="10629-147">Hadoop streaming communicates with hello mapper and reducer over STDIN and STDOUT.</span></span> <span data-ttu-id="10629-148">hello leképező nyomáscsökkentő STDIN egyszerre egy sor-adatok olvasása és írása hello kimeneti tooSTDOUT.</span><span class="sxs-lookup"><span data-stu-id="10629-148">hello mapper and reducer read data a line at a time from STDIN, and write hello output tooSTDOUT.</span></span> <span data-ttu-id="10629-149">Minden sor olvasása vagy hello hozzárendelést és nyomáscsökkentő által kibocsátott kulcs/érték pár, tabulátor elválasztott hello formátumban kell megadni:</span><span class="sxs-lookup"><span data-stu-id="10629-149">Each line read or emitted by hello mapper and reducer must be in hello format of a key/value pair, delimited by a tab character:</span></span>

    [key]/t[value]

<span data-ttu-id="10629-150">További információkért lásd: [Hadoop Streamelési](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span><span class="sxs-lookup"><span data-stu-id="10629-150">For more information, see [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span></span>

<span data-ttu-id="10629-151">Hadoop-Stream és a HDInsight együttes használatával, tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="10629-151">For examples of using Hadoop streaming with HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="10629-152">C# MapReduce-feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="10629-152">Develop C# MapReduce jobs</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="10629-153">Python-MapReduce-feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="10629-153">Develop Python MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="10629-154"><a id="data"></a>Példa adatok</span><span class="sxs-lookup"><span data-stu-id="10629-154"><a id="data"></a>Example data</span></span>

<span data-ttu-id="10629-155">A HDInsight lehetővé különböző példa adatkészletek, hello tárolt `/example/data` és `/HdiSamples` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="10629-155">HDInsight provides various example data sets, which are stored in hello `/example/data` and `/HdiSamples` directory.</span></span> <span data-ttu-id="10629-156">Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="10629-156">These directories are in hello default storage for your cluster.</span></span> <span data-ttu-id="10629-157">Ebben a dokumentumban hello használjuk `/example/data/gutenberg/davinci.txt` fájlt.</span><span class="sxs-lookup"><span data-stu-id="10629-157">In this document, we use hello `/example/data/gutenberg/davinci.txt` file.</span></span> <span data-ttu-id="10629-158">Ez a fájl tartalmazza a Leonardo Da Vinci hello notebookok.</span><span class="sxs-lookup"><span data-stu-id="10629-158">This file contains hello notebooks of Leonardo Da Vinci.</span></span>

## <span data-ttu-id="10629-159"><a id="job"></a>Példa MapReduce</span><span class="sxs-lookup"><span data-stu-id="10629-159"><a id="job"></a>Example MapReduce</span></span>

<span data-ttu-id="10629-160">MapReduce word-count alkalmazás például a HDInsight-fürt része.</span><span class="sxs-lookup"><span data-stu-id="10629-160">An example MapReduce word count application is included with your HDInsight cluster.</span></span> <span data-ttu-id="10629-161">Ebben a példában a következő helyen található `/example/jars/hadoop-mapreduce-examples.jar` hello alapértelmezett tároló a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="10629-161">This example is located at `/example/jars/hadoop-mapreduce-examples.jar` on hello default storage for your cluster.</span></span>

<span data-ttu-id="10629-162">a következő Java-kóddal hello hello hello szereplő MapReduce-alkalmazás forrását hello `hadoop-mapreduce-examples.jar` fájlt:</span><span class="sxs-lookup"><span data-stu-id="10629-162">hello following Java code is hello source of hello MapReduce application contained in hello `hadoop-mapreduce-examples.jar` file:</span></span>

```java
package org.apache.hadoop.examples;

import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

    public static class TokenizerMapper
        extends Mapper<Object, Text, Text, IntWritable>{

    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();

    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
        StringTokenizer itr = new StringTokenizer(value.toString());
        while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
        }
    }
    }

    public static class IntSumReducer
        extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values,
                        Context context
                        ) throws IOException, InterruptedException {
        int sum = 0;
        for (IntWritable val : values) {
        sum += val.get();
        }
        result.set(sum);
        context.write(key, result);
    }
    }

    public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length != 2) {
        System.err.println("Usage: wordcount <in> <out>");
        System.exit(2);
    }
    Job job = new Job(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    FileInputFormat.addInputPath(job, new Path(otherArgs[0]));
    FileOutputFormat.setOutputPath(job, new Path(otherArgs[1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
    }
}
```

<span data-ttu-id="10629-163">Utasításokat toowrite saját MapReduce alkalmazások, tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="10629-163">For instructions toowrite your own MapReduce applications, see hello following documents:</span></span>

* [<span data-ttu-id="10629-164">A HDInsight Java MapReduce-alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="10629-164">Develop Java MapReduce applications for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="10629-165">A HDInsight Python MapReduce alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="10629-165">Develop Python MapReduce applications for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="10629-166"><a id="run"></a>Hello MapReduce futtatása</span><span class="sxs-lookup"><span data-stu-id="10629-166"><a id="run"></a>Run hello MapReduce</span></span>

<span data-ttu-id="10629-167">A HDInsight HiveQL feladatok futtatásához különböző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="10629-167">HDInsight can run HiveQL jobs by using various methods.</span></span> <span data-ttu-id="10629-168">Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="10629-168">Use hello following table toodecide which method is right for you, then follow hello link for a walkthrough.</span></span>

| <span data-ttu-id="10629-169">**Ezzel**...</span><span class="sxs-lookup"><span data-stu-id="10629-169">**Use this**...</span></span> | <span data-ttu-id="10629-170">**.. .toodo Ez**</span><span class="sxs-lookup"><span data-stu-id="10629-170">**...toodo this**</span></span> | <span data-ttu-id="10629-171">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="10629-171">...with this **cluster operating system**</span></span> | <span data-ttu-id="10629-172">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="10629-172">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="10629-173">SSH</span><span class="sxs-lookup"><span data-stu-id="10629-173">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="10629-174">Hello Hadoop paranccsal keresztül **SSH**</span><span class="sxs-lookup"><span data-stu-id="10629-174">Use hello Hadoop command through **SSH**</span></span> |<span data-ttu-id="10629-175">Linux</span><span class="sxs-lookup"><span data-stu-id="10629-175">Linux</span></span> |<span data-ttu-id="10629-176">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-176">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="10629-177">Curl</span><span class="sxs-lookup"><span data-stu-id="10629-177">Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="10629-178">Hello feladat elküldése távolról használatával **REST**</span><span class="sxs-lookup"><span data-stu-id="10629-178">Submit hello job remotely by using **REST**</span></span> |<span data-ttu-id="10629-179">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-179">Linux or Windows</span></span> |<span data-ttu-id="10629-180">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="10629-181">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="10629-181">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="10629-182">Hello feladat elküldése távolról használatával **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="10629-182">Submit hello job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="10629-183">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="10629-183">Linux or Windows</span></span> |<span data-ttu-id="10629-184">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-184">Windows</span></span> |
| <span data-ttu-id="10629-185">[A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="10629-185">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="10629-186">Hello Hadoop paranccsal keresztül **távoli asztal**</span><span class="sxs-lookup"><span data-stu-id="10629-186">Use hello Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="10629-187">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-187">Windows</span></span> |<span data-ttu-id="10629-188">Windows</span><span class="sxs-lookup"><span data-stu-id="10629-188">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="10629-189">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="10629-189">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="10629-190">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="10629-190">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="10629-191"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10629-191"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="10629-192">További információk a HDInsight-adatokkal végzett munka toolearn tekintse meg a következő dokumentumok hello:</span><span class="sxs-lookup"><span data-stu-id="10629-192">toolearn more about working with data in HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="10629-193">Java-MapReduce programok fejlesztése a HDInsight</span><span class="sxs-lookup"><span data-stu-id="10629-193">Develop Java MapReduce programs for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="10629-194">A HDInsight MapReduce programok streaming Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="10629-194">Develop Python streaming MapReduce programs for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

* [<span data-ttu-id="10629-195">A HDInsight az Apache Hadoop MapReduce forrázást feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="10629-195">Develop Scalding MapReduce jobs with Apache Hadoop on HDInsight</span></span>](hdinsight-hadoop-mapreduce-scalding.md)

* <span data-ttu-id="10629-196">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="10629-196">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>

* <span data-ttu-id="10629-197">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="10629-197">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
