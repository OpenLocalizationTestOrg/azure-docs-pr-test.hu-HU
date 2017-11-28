---
title: A hdinsight Hadoop MapReduce |} Microsoft Docs
description: "Útmutató a Hadoop MapReduce-feladatok futtatása a HDInsight-fürtök."
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
ms.openlocfilehash: df8ac578a56de72df667b1fa7f90f981c79d9999
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a><span data-ttu-id="25ac2-103">A HDInsight Hadoop MapReduce használata</span><span class="sxs-lookup"><span data-stu-id="25ac2-103">Use MapReduce in Hadoop on HDInsight</span></span>

<span data-ttu-id="25ac2-104">Útmutató a HDInsight-fürtökön MapReduce-feladatok futtatásához.</span><span class="sxs-lookup"><span data-stu-id="25ac2-104">Learn how to run MapReduce jobs on HDInsight clusters.</span></span> <span data-ttu-id="25ac2-105">A különböző módszereket, hogy használható-e a MapReduce felderítése a következő táblázat segítségével a hdinsight eszközzel:</span><span class="sxs-lookup"><span data-stu-id="25ac2-105">Use the following table to discover the various ways that MapReduce can be used with HDInsight:</span></span>

| <span data-ttu-id="25ac2-106">**Ezzel**...</span><span class="sxs-lookup"><span data-stu-id="25ac2-106">**Use this**...</span></span> | <span data-ttu-id="25ac2-107">**.. fenti ehhez**</span><span class="sxs-lookup"><span data-stu-id="25ac2-107">**...to do this**</span></span> | <span data-ttu-id="25ac2-108">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="25ac2-108">...with this **cluster operating system**</span></span> | <span data-ttu-id="25ac2-109">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="25ac2-109">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="25ac2-110">SSH</span><span class="sxs-lookup"><span data-stu-id="25ac2-110">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="25ac2-111">A Hadoop paranccsal keresztül **SSH**</span><span class="sxs-lookup"><span data-stu-id="25ac2-111">Use the Hadoop command through **SSH**</span></span> |<span data-ttu-id="25ac2-112">Linux</span><span class="sxs-lookup"><span data-stu-id="25ac2-112">Linux</span></span> |<span data-ttu-id="25ac2-113">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-113">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="25ac2-114">REST</span><span class="sxs-lookup"><span data-stu-id="25ac2-114">REST</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="25ac2-115">A feladat elküldéséhez távolról használatával **REST** (példák használata cURL)</span><span class="sxs-lookup"><span data-stu-id="25ac2-115">Submit the job remotely by using **REST** (examples use cURL)</span></span> |<span data-ttu-id="25ac2-116">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-116">Linux or Windows</span></span> |<span data-ttu-id="25ac2-117">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-117">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="25ac2-118">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="25ac2-118">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="25ac2-119">A feladat elküldéséhez távolról használatával **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="25ac2-119">Submit the job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="25ac2-120">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-120">Linux or Windows</span></span> |<span data-ttu-id="25ac2-121">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-121">Windows</span></span> |
| <span data-ttu-id="25ac2-122">[A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="25ac2-122">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="25ac2-123">A Hadoop paranccsal keresztül **távoli asztal**</span><span class="sxs-lookup"><span data-stu-id="25ac2-123">Use the Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="25ac2-124">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-124">Windows</span></span> |<span data-ttu-id="25ac2-125">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-125">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="25ac2-126">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="25ac2-126">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="25ac2-127">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="25ac2-127">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="25ac2-128"><a id="whatis"></a>Mi az a MapReduce</span><span class="sxs-lookup"><span data-stu-id="25ac2-128"><a id="whatis"></a>What is MapReduce</span></span>

<span data-ttu-id="25ac2-129">Hadoop-MapReduce egy szoftveres keretrendszer feladatok, amelyek feldolgozzák a nagy mennyiségű adat írásához.</span><span class="sxs-lookup"><span data-stu-id="25ac2-129">Hadoop MapReduce is a software framework for writing jobs that process vast amounts of data.</span></span> <span data-ttu-id="25ac2-130">A bemeneti adatok független adattömböket, majd párhuzamosan futnak a fürt csomópontjai között oszlik meg.</span><span class="sxs-lookup"><span data-stu-id="25ac2-130">Input data is split into independent chunks, which are then processed in parallel across the nodes in your cluster.</span></span> <span data-ttu-id="25ac2-131">A MapReduce feladatot két funkciókat foglalja magában:</span><span class="sxs-lookup"><span data-stu-id="25ac2-131">A MapReduce job consists of two functions:</span></span>

* <span data-ttu-id="25ac2-132">**Eseményleképező**: bemeneti adatforrástól, elemzi azokat (általában a szűrési és rendezési műveletek) és bocsát ki a listának (kulcs-érték párok)</span><span class="sxs-lookup"><span data-stu-id="25ac2-132">**Mapper**: Consumes input data, analyzes it (usually with filter and sorting operations), and emits tuples (key-value pairs)</span></span>

* <span data-ttu-id="25ac2-133">**Nyomáscsökkentő**: a leképező által kibocsátott rekordokat használ fel, és hozza létre a kisebb, kombinált eredményeként a leképező adatokból összefoglaló műveletet hajt végre</span><span class="sxs-lookup"><span data-stu-id="25ac2-133">**Reducer**: Consumes tuples emitted by the Mapper and performs a summary operation that creates a smaller, combined result from the Mapper data</span></span>

<span data-ttu-id="25ac2-134">Egy alapszintű word száma MapReduce feladatot például az alábbi ábrán látható:</span><span class="sxs-lookup"><span data-stu-id="25ac2-134">A basic word count MapReduce job example is illustrated in the following diagram:</span></span>

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

<span data-ttu-id="25ac2-136">Ez a feladat eredménye hányszor minden szó történt a szöveg, amelynek meg lett számát.</span><span class="sxs-lookup"><span data-stu-id="25ac2-136">The output of this job is a count of how many times each word occurred in the text that was analyzed.</span></span>

* <span data-ttu-id="25ac2-137">A leképező bemeneti adatokként a bemeneti szöveg soronként vesz igénybe, és elindítja azt szavakat.</span><span class="sxs-lookup"><span data-stu-id="25ac2-137">The mapper takes each line from the input text as an input and breaks it into words.</span></span> <span data-ttu-id="25ac2-138">Az megfelelően kibocsát egy kulcs/érték pár minden alkalommal, amikor egy word akkor fordul elő, a Word követi egy 1.</span><span class="sxs-lookup"><span data-stu-id="25ac2-138">It emits a key/value pair each time a word occurs of the word is followed by a 1.</span></span> <span data-ttu-id="25ac2-139">A kimeneti rendezése nyomáscsökkentő felé.</span><span class="sxs-lookup"><span data-stu-id="25ac2-139">The output is sorted before sending it to reducer.</span></span>
* <span data-ttu-id="25ac2-140">A nyomáscsökkentő ezeket a minden egyes szó egyedi számokat összegzi, és megfelelően kibocsát egy egyetlen kulcs/érték pár, amely tartalmazza a word, az előfordulások összege követ.</span><span class="sxs-lookup"><span data-stu-id="25ac2-140">The reducer sums these individual counts for each word and emits a single key/value pair that contains the word followed by the sum of its occurrences.</span></span>

<span data-ttu-id="25ac2-141">MapReduce különböző nyelveken valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="25ac2-141">MapReduce can be implemented in various languages.</span></span> <span data-ttu-id="25ac2-142">Java leggyakoribb megvalósítása, és a jelen dokumentum bemutatási célokra szolgál.</span><span class="sxs-lookup"><span data-stu-id="25ac2-142">Java is the most common implementation, and is used for demonstration purposes in this document.</span></span>

## <a name="development-languages"></a><span data-ttu-id="25ac2-143">Fejlesztési nyelveket</span><span class="sxs-lookup"><span data-stu-id="25ac2-143">Development languages</span></span>

<span data-ttu-id="25ac2-144">Nyelvet vagy keretrendszert, Java és a Java virtuális gépen alapuló közvetlenül, a MapReduce feladatot is futott.</span><span class="sxs-lookup"><span data-stu-id="25ac2-144">Languages or frameworks that are based on Java and the Java Virtual Machine can be ran directly as a MapReduce job.</span></span> <span data-ttu-id="25ac2-145">Ebben a dokumentumban bemutatott példában egy Java-MapReduce-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="25ac2-145">The example used in this document is a Java MapReduce application.</span></span> <span data-ttu-id="25ac2-146">Nem-Java nyelven, például a C#, Python vagy önálló végrehajtható fájlok, a Hadoop streamelési kell használnia.</span><span class="sxs-lookup"><span data-stu-id="25ac2-146">Non-Java languages, such as C#, Python, or standalone executables, must use Hadoop streaming.</span></span>

<span data-ttu-id="25ac2-147">Hadoop streamelési keresztül kommunikál a hozzárendelést és nyomáscsökkentő STDIN és a STDOUT.</span><span class="sxs-lookup"><span data-stu-id="25ac2-147">Hadoop streaming communicates with the mapper and reducer over STDIN and STDOUT.</span></span> <span data-ttu-id="25ac2-148">A leképező nyomáscsökkentő STDIN egyszerre egy sor adatokat olvasni és írni a kimeneti STDOUT.</span><span class="sxs-lookup"><span data-stu-id="25ac2-148">The mapper and reducer read data a line at a time from STDIN, and write the output to STDOUT.</span></span> <span data-ttu-id="25ac2-149">Minden sor olvasása vagy a hozzárendelést és nyomáscsökkentő által kibocsátott tabulátor határolt egy kulcs/érték pár formátumban kell lennie:</span><span class="sxs-lookup"><span data-stu-id="25ac2-149">Each line read or emitted by the mapper and reducer must be in the format of a key/value pair, delimited by a tab character:</span></span>

    [key]/t[value]

<span data-ttu-id="25ac2-150">További információkért lásd: [Hadoop Streamelési](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span><span class="sxs-lookup"><span data-stu-id="25ac2-150">For more information, see [Hadoop Streaming](http://hadoop.apache.org/docs/r1.2.1/streaming.html).</span></span>

<span data-ttu-id="25ac2-151">Hadoop-Stream és a HDInsight együttes használatával tekintse meg a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="25ac2-151">For examples of using Hadoop streaming with HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="25ac2-152">C# MapReduce-feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="25ac2-152">Develop C# MapReduce jobs</span></span>](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [<span data-ttu-id="25ac2-153">Python-MapReduce-feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="25ac2-153">Develop Python MapReduce jobs</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="25ac2-154"><a id="data"></a>Példa adatok</span><span class="sxs-lookup"><span data-stu-id="25ac2-154"><a id="data"></a>Example data</span></span>

<span data-ttu-id="25ac2-155">A HDInsight lehetővé különböző példa adatkészletek, amelyek tárolása a `/example/data` és `/HdiSamples` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="25ac2-155">HDInsight provides various example data sets, which are stored in the `/example/data` and `/HdiSamples` directory.</span></span> <span data-ttu-id="25ac2-156">Ezeket a könyvtárakat az alapértelmezett tároló, a fürt szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="25ac2-156">These directories are in the default storage for your cluster.</span></span> <span data-ttu-id="25ac2-157">Ebben a dokumentumban használjuk a `/example/data/gutenberg/davinci.txt` fájlt.</span><span class="sxs-lookup"><span data-stu-id="25ac2-157">In this document, we use the `/example/data/gutenberg/davinci.txt` file.</span></span> <span data-ttu-id="25ac2-158">Ez a fájl Leonardo Da Vinci jegyzetfüzet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="25ac2-158">This file contains the notebooks of Leonardo Da Vinci.</span></span>

## <span data-ttu-id="25ac2-159"><a id="job"></a>Példa MapReduce</span><span class="sxs-lookup"><span data-stu-id="25ac2-159"><a id="job"></a>Example MapReduce</span></span>

<span data-ttu-id="25ac2-160">MapReduce word-count alkalmazás például a HDInsight-fürt része.</span><span class="sxs-lookup"><span data-stu-id="25ac2-160">An example MapReduce word count application is included with your HDInsight cluster.</span></span> <span data-ttu-id="25ac2-161">Ebben a példában a következő helyen található `/example/jars/hadoop-mapreduce-examples.jar` az alapértelmezett tároló, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="25ac2-161">This example is located at `/example/jars/hadoop-mapreduce-examples.jar` on the default storage for your cluster.</span></span>

<span data-ttu-id="25ac2-162">A következő Java-kódot egy a MapReduce alkalmazás szerepel a `hadoop-mapreduce-examples.jar` fájlt:</span><span class="sxs-lookup"><span data-stu-id="25ac2-162">The following Java code is the source of the MapReduce application contained in the `hadoop-mapreduce-examples.jar` file:</span></span>

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

<span data-ttu-id="25ac2-163">A saját MapReduce alkalmazások írását, lásd: a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="25ac2-163">For instructions to write your own MapReduce applications, see the following documents:</span></span>

* [<span data-ttu-id="25ac2-164">A HDInsight Java MapReduce-alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="25ac2-164">Develop Java MapReduce applications for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="25ac2-165">A HDInsight Python MapReduce alkalmazások fejlesztéséhez</span><span class="sxs-lookup"><span data-stu-id="25ac2-165">Develop Python MapReduce applications for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

## <span data-ttu-id="25ac2-166"><a id="run"></a>A MapReduce futtatása</span><span class="sxs-lookup"><span data-stu-id="25ac2-166"><a id="run"></a>Run the MapReduce</span></span>

<span data-ttu-id="25ac2-167">A HDInsight HiveQL feladatok futtatásához különböző módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="25ac2-167">HDInsight can run HiveQL jobs by using various methods.</span></span> <span data-ttu-id="25ac2-168">A következő táblázat segítségével döntse el, melyik módszert részesíti az Ön számára legmegfelelőbb, majd kövesse a hivatkozást útmutatást.</span><span class="sxs-lookup"><span data-stu-id="25ac2-168">Use the following table to decide which method is right for you, then follow the link for a walkthrough.</span></span>

| <span data-ttu-id="25ac2-169">**Ezzel**...</span><span class="sxs-lookup"><span data-stu-id="25ac2-169">**Use this**...</span></span> | <span data-ttu-id="25ac2-170">**.. fenti ehhez**</span><span class="sxs-lookup"><span data-stu-id="25ac2-170">**...to do this**</span></span> | <span data-ttu-id="25ac2-171">és mivel ez **fürt operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="25ac2-171">...with this **cluster operating system**</span></span> | <span data-ttu-id="25ac2-172">.. .from ez **ügyfél operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="25ac2-172">...from this **client operating system**</span></span> |
|:--- |:--- |:--- |:--- |
| [<span data-ttu-id="25ac2-173">SSH</span><span class="sxs-lookup"><span data-stu-id="25ac2-173">SSH</span></span>](hdinsight-hadoop-use-mapreduce-ssh.md) |<span data-ttu-id="25ac2-174">A Hadoop paranccsal keresztül **SSH**</span><span class="sxs-lookup"><span data-stu-id="25ac2-174">Use the Hadoop command through **SSH**</span></span> |<span data-ttu-id="25ac2-175">Linux</span><span class="sxs-lookup"><span data-stu-id="25ac2-175">Linux</span></span> |<span data-ttu-id="25ac2-176">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-176">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="25ac2-177">Curl</span><span class="sxs-lookup"><span data-stu-id="25ac2-177">Curl</span></span>](hdinsight-hadoop-use-mapreduce-curl.md) |<span data-ttu-id="25ac2-178">A feladat elküldéséhez távolról használatával **REST**</span><span class="sxs-lookup"><span data-stu-id="25ac2-178">Submit the job remotely by using **REST**</span></span> |<span data-ttu-id="25ac2-179">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-179">Linux or Windows</span></span> |<span data-ttu-id="25ac2-180">Linux, Unix, Mac OS X vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-180">Linux, Unix, Mac OS X, or Windows</span></span> |
| [<span data-ttu-id="25ac2-181">A Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="25ac2-181">Windows PowerShell</span></span>](hdinsight-hadoop-use-mapreduce-powershell.md) |<span data-ttu-id="25ac2-182">A feladat elküldéséhez távolról használatával **Windows PowerShell**</span><span class="sxs-lookup"><span data-stu-id="25ac2-182">Submit the job remotely by using **Windows PowerShell**</span></span> |<span data-ttu-id="25ac2-183">Linux- vagy Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-183">Linux or Windows</span></span> |<span data-ttu-id="25ac2-184">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-184">Windows</span></span> |
| <span data-ttu-id="25ac2-185">[A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as)</span><span class="sxs-lookup"><span data-stu-id="25ac2-185">[Remote Desktop](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2 and 3.3)</span></span> |<span data-ttu-id="25ac2-186">A Hadoop paranccsal keresztül **távoli asztal**</span><span class="sxs-lookup"><span data-stu-id="25ac2-186">Use the Hadoop command through **Remote Desktop**</span></span> |<span data-ttu-id="25ac2-187">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-187">Windows</span></span> |<span data-ttu-id="25ac2-188">Windows</span><span class="sxs-lookup"><span data-stu-id="25ac2-188">Windows</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="25ac2-189">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="25ac2-189">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="25ac2-190">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="25ac2-190">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <span data-ttu-id="25ac2-191"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="25ac2-191"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="25ac2-192">A HDInsight-adatokkal végzett munka kapcsolatos további tudnivalókért tekintse meg a következő dokumentumokat:</span><span class="sxs-lookup"><span data-stu-id="25ac2-192">To learn more about working with data in HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="25ac2-193">Java-MapReduce programok fejlesztése a HDInsight</span><span class="sxs-lookup"><span data-stu-id="25ac2-193">Develop Java MapReduce programs for HDInsight</span></span>](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [<span data-ttu-id="25ac2-194">A HDInsight MapReduce programok streaming Python fejlesztése</span><span class="sxs-lookup"><span data-stu-id="25ac2-194">Develop Python streaming MapReduce programs for HDInsight</span></span>](hdinsight-hadoop-streaming-python.md)

* [<span data-ttu-id="25ac2-195">A HDInsight az Apache Hadoop MapReduce forrázást feladatok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="25ac2-195">Develop Scalding MapReduce jobs with Apache Hadoop on HDInsight</span></span>](hdinsight-hadoop-mapreduce-scalding.md)

* <span data-ttu-id="25ac2-196">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="25ac2-196">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>

* <span data-ttu-id="25ac2-197">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="25ac2-197">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
