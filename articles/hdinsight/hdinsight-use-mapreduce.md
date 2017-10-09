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
# <a name="use-mapreduce-in-hadoop-on-hdinsight"></a>A HDInsight Hadoop MapReduce használata

Ismerje meg, hogyan toorun MapReduce feladatok a HDInsight-fürtökön. A következő tábla toodiscover hello módszert arról, hogy használható-e a MapReduce with HDInsight hello használata:

| **Ezzel**... | **.. .toodo Ez** | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél operációs rendszer** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Hello Hadoop paranccsal keresztül **SSH** |Linux |Linux, Unix, Mac OS X vagy Windows |
| [REST](hdinsight-hadoop-use-mapreduce-curl.md) |Hello feladat elküldése távolról használatával **REST** (példák használata cURL) |Linux- vagy Windows |Linux, Unix, Mac OS X vagy Windows |
| [A Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Hello feladat elküldése távolról használatával **Windows PowerShell** |Linux- vagy Windows |Windows |
| [A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as) |Hello Hadoop paranccsal keresztül **távoli asztal** |Windows |Windows |

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="whatis"></a>Mi az a MapReduce

Hadoop-MapReduce egy szoftveres keretrendszer feladatok, amelyek feldolgozzák a nagy mennyiségű adat írásához. A bemeneti adatok független adattömböket, majd párhuzamosan futnak hello a fürtben található csomópontok között oszlik meg. A MapReduce feladatot két funkciókat foglalja magában:

* **Eseményleképező**: bemeneti adatforrástól, elemzi azokat (általában a szűrési és rendezési műveletek) és bocsát ki a listának (kulcs-érték párok)

* **Nyomáscsökkentő**: hello leképező által kibocsátott rekordokat használ fel, és hozza létre a kisebb, kombinált eredményeként a hello leképező adatok összegzési műveletet hajt végre

Egy alapszintű word száma MapReduce feladatot példa mutatja be a következő diagram hello:

![HDI. WordCountDiagram][image-hdi-wordcountdiagram]

hello a feladat eredménye hányszor hello szöveg, amelynek történt meg minden szó történt számát.

* hello leképező minden sor a bemeneti szöveg hello bemenetként vesz igénybe, és elindítja azt szavakat. Az megfelelően kibocsát egy kulcs/érték pár minden alkalommal, amikor egy word akkor fordul elő, a hello szó a egy 1 követ. hello kimeneti tooreducer elküldés előtt van rendezve.
* hello nyomáscsökkentő ezeket a minden egyes szó egyedi számokat összegzi, és megfelelően kibocsát egy egyetlen kulcs/érték pár hello szó, és az előfordulások hello összegét tartalmazó.

MapReduce különböző nyelveken valósítható meg. Java hello legelterjedtebb, és a jelen dokumentum bemutatási célokra szolgál.

## <a name="development-languages"></a>Fejlesztési nyelveket

Nyelvet vagy keretrendszert, amely Java alapulnak, és a Java virtuális gép hello közvetlenül, a MapReduce feladatot is futott. hello itt példája egy Java-MapReduce-alkalmazást. Nem-Java nyelven, például a C#, Python vagy önálló végrehajtható fájlok, a Hadoop streamelési kell használnia.

Hadoop streamelési keresztül kommunikál a hello leképező és nyomáscsökkentő STDIN és a STDOUT. hello leképező nyomáscsökkentő STDIN egyszerre egy sor-adatok olvasása és írása hello kimeneti tooSTDOUT. Minden sor olvasása vagy hello hozzárendelést és nyomáscsökkentő által kibocsátott kulcs/érték pár, tabulátor elválasztott hello formátumban kell megadni:

    [key]/t[value]

További információkért lásd: [Hadoop Streamelési](http://hadoop.apache.org/docs/r1.2.1/streaming.html).

Hadoop-Stream és a HDInsight együttes használatával, tekintse meg a következő dokumentumok hello:

* [C# MapReduce-feladatok fejlesztése](hdinsight-hadoop-dotnet-csharp-mapreduce-streaming.md)

* [Python-MapReduce-feladatok fejlesztése](hdinsight-hadoop-streaming-python.md)

## <a id="data"></a>Példa adatok

A HDInsight lehetővé különböző példa adatkészletek, hello tárolt `/example/data` és `/HdiSamples` könyvtár. Ezeket a könyvtárakat hello alapértelmezett tárolási a fürt szerepelnek. Ebben a dokumentumban hello használjuk `/example/data/gutenberg/davinci.txt` fájlt. Ez a fájl tartalmazza a Leonardo Da Vinci hello notebookok.

## <a id="job"></a>Példa MapReduce

MapReduce word-count alkalmazás például a HDInsight-fürt része. Ebben a példában a következő helyen található `/example/jars/hadoop-mapreduce-examples.jar` hello alapértelmezett tároló a fürt számára.

a következő Java-kóddal hello hello hello szereplő MapReduce-alkalmazás forrását hello `hadoop-mapreduce-examples.jar` fájlt:

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

Utasításokat toowrite saját MapReduce alkalmazások, tekintse meg a következő dokumentumok hello:

* [A HDInsight Java MapReduce-alkalmazások fejlesztéséhez](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [A HDInsight Python MapReduce alkalmazások fejlesztéséhez](hdinsight-hadoop-streaming-python.md)

## <a id="run"></a>Hello MapReduce futtatása

A HDInsight HiveQL feladatok futtatásához különböző módszerekkel. Használja a következő tábla toodecide, melyik módszert részesíti az Ön számára legmegfelelőbb hello, majd kövesse az útmutatást hello hivatkozásra.

| **Ezzel**... | **.. .toodo Ez** | és mivel ez **fürt operációs rendszer** | .. .from ez **ügyfél operációs rendszer** |
|:--- |:--- |:--- |:--- |
| [SSH](hdinsight-hadoop-use-mapreduce-ssh.md) |Hello Hadoop paranccsal keresztül **SSH** |Linux |Linux, Unix, Mac OS X vagy Windows |
| [Curl](hdinsight-hadoop-use-mapreduce-curl.md) |Hello feladat elküldése távolról használatával **REST** |Linux- vagy Windows |Linux, Unix, Mac OS X vagy Windows |
| [A Windows PowerShell](hdinsight-hadoop-use-mapreduce-powershell.md) |Hello feladat elküldése távolról használatával **Windows PowerShell** |Linux- vagy Windows |Windows |
| [A távoli asztal](hdinsight-hadoop-use-mapreduce-remote-desktop.md) (HDInsight 3.2-es és 3.3-as) |Hello Hadoop paranccsal keresztül **távoli asztal** |Windows |Windows |

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="nextsteps"></a>Következő lépések

További információk a HDInsight-adatokkal végzett munka toolearn tekintse meg a következő dokumentumok hello:

* [Java-MapReduce programok fejlesztése a HDInsight](hdinsight-develop-deploy-java-mapreduce-linux.md)

* [A HDInsight MapReduce programok streaming Python fejlesztése](hdinsight-hadoop-streaming-python.md)

* [A HDInsight az Apache Hadoop MapReduce forrázást feladatok fejlesztése](hdinsight-hadoop-mapreduce-scalding.md)

* [A Hive használata a HDInsightban][hdinsight-use-hive]

* [A Pig használata a HDInsightban][hdinsight-use-pig]


[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md
[hdinsight-develop-mapreduce-jobs]: hdinsight-develop-deploy-java-mapreduce-linux.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md


[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[image-hdi-wordcountdiagram]: ./media/hdinsight-use-mapreduce/HDI.WordCountDiagram.gif
