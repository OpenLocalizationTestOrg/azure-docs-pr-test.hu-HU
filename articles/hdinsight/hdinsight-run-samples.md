---
title: "aaaRun hello Hadoop-minták a HDInsight - Azure |} Microsoft Docs"
description: "A megadott hello minták hello Azure HDInsight-szolgáltatás az első lépéseiben. Amely MapReduce programok adatok fürtökön PowerShell-parancsfájlok használata."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: bf76d452-abb4-4210-87bd-a2067778c6ed
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 544856a2cdfe5154cbd9bf1fb05db081af86cd46
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="run-hadoop-mapreduce-samples-in-windows-based-hdinsight"></a>Windows-alapú HDInsight Hadoop-MapReduce minták futtatása
[!INCLUDE [samples-selector](../../includes/hdinsight-run-samples-selector.md)]

Minták toohelp hozzáfogna MapReduce-feladatok futó Azure HDInsight Hadoop-fürtök rendelkezésre állnak. Ezeket a mintákat elérhetővé válnak az egyes hello HDInsight felügyelt fürtöt, az Ön által létrehozott. Ezek a minták futtatása megismerkedhet az Azure PowerShell-parancsmagok toorun feladatok a Hadoop-fürtök a.

* [**Word-count**][hdinsight-sample-wordcount]: szövegfájl word előfordulások száma.
* [**C# szószámot streaming**][hdinsight-sample-csharp-streaming]: számok word előfordulás használatával szöveges fájlt hello Hadoop-Stream felületet.
* [**A pi négyzetgyökének**][hdinsight-sample-pi-estimator]: használja a statisztikai (látszólagos Monte Carlo) metódus tooestimate hello a pi értékét.
* [**10 GB-os Graysort**][hdinsight-sample-10gb-graysort]: egy általános célú GraySort futtatnak egy 10 GB-os fájl HDInsight használatával. Nincsenek a három feladatok toorun: Teragen toogenerate hello adatok Terasort toosort hello adatok és Teravalidate tooconfirm, hogy hello adatok megfelelően lett rendezve.

> [!NOTE]
> hello forráskód hello függelék találhatók.

Sokkal további dokumentáció megtalálható a Hadoop kapcsolatos technológiák, például a Java-alapú MapReduce programozási és adatfolyam- és használt Windows PowerShell parancsmagok hello vonatkozó dokumentációt hello webes parancsfájlok. Ezekkel az erőforrásokkal kapcsolatos további információkért lásd:

* [A hdinsight Hadoop Java MapReduce programok fejlesztése](hdinsight-develop-deploy-java-mapreduce-linux.md)
* [Hadoop-feladatok elküldése a HDInsightban](hdinsight-submit-hadoop-jobs-programmatically.md)
* [A HDInsight bemutatása tooAzure][hdinsight-introduction]

Napjainkban sokan válassza a Hive és a Pig MapReduce keresztül.  További információkért lásd:

* [A Hive hdinsight használata](hdinsight-use-hive.md)
* [A Pig használata a Hdinsightban](hdinsight-use-pig.md)

**Előfeltételek**:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **HDInsight-fürtök**. Hello különböző módokon, amelyben a fürtök hozhatók létre, lásd: [Hadoop létrehozása a HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md).
* **Munkaállomás Azure PowerShell-lel**.

    > [!IMPORTANT]
    > A HDInsight-erőforrások Azure Service Managerrel történő kezelésének Azure PowerShell-támogatása **elavult**, így 2017. január 1-től megszűnik. a dokumentum használatát hello új HDInsight-parancsmagokat, amelyek működnek az Azure Resource Manager hello szükséges lépések.
    >
    > Hello kövesse [telepítse és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs) tooinstall hello legújabb verziót. Ha vannak olyan parancsprogramjai, hogy szükség toobe módosított toouse hello új parancsmagok használata Azure Resource Managerrel működő című [áttelepítése tooAzure fejlesztési Resource Manager-alapú HDInsight-fürtök eszközei](hdinsight-hadoop-development-using-azure-resource-manager.md).

## <a name="hdinsight-sample-wordcount"></a>Word-count - Java
a MapReduce projekt toosubmit, először létre kell hoznia egy MapReduce feladat definíciójához. A hello feladatdefiníció, akkor adja meg hello MapReduce program jar-fájlra és hello jar-fájlra, amely hello helyét **wasb:///example/jars/hadoop-mapreduce-examples.jar**osztálynév hello és hello argumentumokat.  hello wordcount MapReduce program két argumentummal: hello forrásfájl használt toocount szavak és kimeneti hello helyét.

hello forráskódját itt található: hello [függelék](#apendix-a---the-word-count-MapReduce-program-in-java).

A Java-MapReduce fejlődő hello eljárás program című - [fejlesztése Java MapReduce programok a Hadoop a Hdinsightban](hdinsight-develop-deploy-java-mapreduce-linux.md)

**a word-count MapReduce feladatot toosubmit**

1. Nyissa meg **Windows PowerShell ISE**. Útmutatásért lásd: [telepítse és konfigurálja az Azure Powershellt][powershell-install-configure].
2. Illessze be a következő PowerShell-parancsfájl hello:

    ```powershell
    $subscriptionName = "<Azure Subscription Name>"
    $resourceGroupName = "<Resource Group Name>"
    $clusterName = "<HDInsight cluster name>"             # HDInsight cluster name

    Select-AzureRmSubscription -SubscriptionName $subscriptionName

    # Define hello MapReduce job
    $mrJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "wordcount" `
                                -Arguments "wasb:///example/data/gutenberg/davinci.txt", "wasb:///example/data/WordCountOutput"

    # Submit hello job and wait for job completion
    $cred = Get-Credential -Message "Enter hello HDInsight cluster HTTP user credential:"
    $mrJob = Start-AzureRmHDInsightJob `
                        -ResourceGroupName $resourceGroupName `
                        -ClusterName $clusterName `
                        -HttpCredential $cred `
                        -JobDefinition $mrJobDefinition

    Wait-AzureRmHDInsightJob `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -JobId $mrJob.JobId

    # Get hello job output
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount)[0].Value
    $defaultStorageContainer = $cluster.DefaultStorageContainer

    Get-AzureRmHDInsightJobOutput `
        -ResourceGroupName $resourceGroupName `
        -ClusterName $clusterName `
        -HttpCredential $cred `
        -DefaultStorageAccountName $defaultStorageAccount `
        -DefaultStorageAccountKey $defaultStorageAccountKey `
        -DefaultContainer $defaultStorageContainer  `
        -JobId $mrJob.JobId `
        -DisplayOutputType StandardError

    # Download hello job output toohello workstation
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob example/data/WordCountOutput/part-r-00000 -Context $storageContext -Force

    # Display hello output file
    cat ./example/data/WordCountOutput/part-r-00000 | findstr "there"
    ```

    hello MapReduce feladatot fájlt hoz létre nevű *rész-r-00000*, amely olyan szavakat tartalmaz, és hello száma. hello parancsfájl a hello **findstr** parancs összes hello toolist olyan szót tartalmaz *"nincs"*.
3. Hello első három változók értékét, és hello parancsprogrammal.

## <a name="hdinsight-sample-csharp-streaming"></a>Word-count - C# adatfolyam
Hadoop streamelési API tooMapReduce, amely lehetővé teszi, hogy a toowrite térkép, és csökkentheti a funkciók nem Java nyelven biztosít.

> [!NOTE]
> Ebben az oktatóanyagban hello lépéseket csak tooWindows-alapú HDInsight-fürtök alkalmazni. Például a Linux-alapú HDInsight-fürtök adatfolyamként való továbbítására, [programok streaming hdinsight fejlesztése Python](hdinsight-hadoop-streaming-python.md).

Hello például hello leképező és hello nyomáscsökkentő vannak végrehajtható fájlok, olvassa el a hello bemenetének [stdin] [ stdin-stdout-stderr] (--soronként) és a kibocsátás túl hello kimeneti[stdout] [stdin-stdout-stderr]. hello program összes hello szavak hello szövegben módszert.

Ha egy végrehajtható fájl van megadva, az **mappers**, minden leképező feladat végrehajtható különálló folyamatként hello indít, amikor hello leképező inicializálva van. Hello leképező feladat fut, a bemeneti alakítja sorok, és hírcsatornák hello sorok toohello [stdin] [ stdin-stdout-stderr] hello folyamat.

A hello addig hello leképező gyűjti össze az hello sor alapú kimeneti hello stdout hello folyamat. Minden egyes sorban alakítja a kulcs/érték pár, amelyek hello leképező hello eredményének gyűjtése történik. Alapértelmezés szerint egy sor mentése toohello első tabulátor hello előtag hello kulcs, és hello maradékát (kivéve a hello tabulátor) hello sor hello érték. Ha hello sor nincs lapon karakter, teljes sor hello kulcs minősül, és hello értéke null.

Ha egy végrehajtható fájl van megadva, az **szűkítő**, minden nyomáscsökkentő feladat végrehajtható különálló folyamatként hello indít, amikor hello nyomáscsökkentő inicializálva van. Hello nyomáscsökkentő feladat fut, a bemeneti kulcs/érték párok alakítja sorok, és azt hírcsatornák hello sorok toohello [stdin] [ stdin-stdout-stderr] hello folyamat.

A hello addig hello nyomáscsökkentő gyűjti össze az hello sor alapú kimeneti hello [stdout] [ stdin-stdout-stderr] hello folyamat. Átalakítja minden sor tooa kulcs/érték pár, amelyek hello nyomáscsökkentő hello eredményének gyűjtése történik. Alapértelmezés szerint egy sor mentése toohello első tabulátor hello előtag hello kulcs, és hello maradékát (kivéve a hello tabulátor) hello sor hello érték.

**toosubmit egy C# adatfolyam word-count feladatot**

* A hello eljárás [Word-count - Java](#word-count-java), és cserélje le a következő sor hello hello feladatdefiníció:

    ```powershell
    $mrJobDefinition = New-AzureRmHDInsightStreamingMapReduceJobDefinition `
                            -Files "/example/apps/cat.exe","/example/apps/wc.exe" `
                            -Mapper "cat.exe" `
                            -Reducer "wc.exe" `
                            -InputPath "/example/data/gutenberg/davinci.txt" `
                            -OutputPath "/example/data/StreamingOutput/wc.txt"
    ```

    hello kimeneti fájl a következők:

        example/data/StreamingOutput/wc.txt/part-00000

## <a name="hdinsight-sample-pi-estimator"></a>A PI négyzetgyökének
hello pi négyzetgyökének használja a statisztikai (látszólagos Monte Carlo) metódus tooestimate hello a pi értékét. Véletlenszerű helyezni egy egység pontok négyzetes is tartoznak hello kör valószínűség egyenlő toohello területtel rendelkező belül, hogy szögletes ütemtervben kör pi/4. a pi értékét hello megbecsülhető hello értékének 4R, ahol R az hello aránya hello hello kör toohello pontok számát, amelyek hello szögletes belül találhatók pontok számát. hello hello kódtöredéket használt pontok, hello jobb hello becsült értéke.

Ez a minta előírt hello parancsfájl egy Hadoop jar feladatot, és van beállítva toorun érték 16 maps, amely egy szükséges toocompute 10 millió minta pontok hello paraméterértékek által. Ezek az értékek lehetnek paraméter tooimprove becsült hello a pi értékét módosítani. Referenciaként hello pi első 10 tizedesjegyre 3.1415926535.

**a pi négyzetgyökének feladat toosubmit**

* A hello eljárás [Word-count - Java](#word-count-java), és cserélje le a következő sor hello hello feladatdefiníció:

    ```powershell
    $mrJobJobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "wasb:///example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "pi" `
                                -Arguments "16", "10000000"
    ```

## <a name="hdinsight-sample-10gb-graysort"></a>10 GB-os Graysort
Ez a minta egy mérsékelt 10GB adatot használ, így viszonylag gyorsan futtatása. Hello MapReduce alkalmazások Owen O'Malley és Arun Murthy, amelyek mellett 0.578 TB/perc (173 percben 100 TB) sebességet 2009 hello éves általános célú ("daytona") terabájt rendezési teljesítményteszt nyert által fejlesztett használ. Erről és más rendezési referenciaalapok további információkért lásd: hello [Sortbenchmark](http://sortbenchmark.org/) hely.

A példa a MapReduce programok három különböző használja:

1. **TeraGen** egy MapReduce program használható adatok toosort toogenerate hello sorát.
2. **TeraSort** hello bemeneti adatokat, és egy teljes rendelés MapReduce toosort hello adatokat használ. TeraSort MapReduce funkciók, kivéve egy egyéni particionáló hello kulcs tartomány minden csökkentse definiálása mintát N-1 kulcsok rendezett listáját használó szabványos rendezési. Ebben az esetben, minden kulcsok ilyen mintát [i-1] < kulcs = < minta [i] küldött tooreduce i. Ez garantálja, hogy hello kimenetének csökkentése i segédanyagokra-nál kisebb hello kimenete csökkentése i + 1.
3. **TeraValidate** , amely ellenőrzi, hogy hello kimeneti MapReduce program globálisan van rendezve. Fájlonként egy leképezést hello kimeneti könyvtár hozna létre, és minden leképezés biztosítja, hogy minden kulcs kisebb vagy egyenlő, mint toohello előzőre. hello térkép függvény is hozza létre hello rekordjának első és utolsó kulcsok minden egyes fájl, és hello csökkentése függvény biztosítja, hogy hello fájl i első kulcsát érték nagyobb, mint a fájl i-1 hello utolsó kulcsa. Hello kimeneteként jelentett problémákat, amelyek nem megfelelő sorrendben hello kulcsokkal csökkentése.

hello bemeneti és kimeneti formátumot, mindhárom alkalmazás által használt beolvassa és hello szövegfájlok hello megfelelő formátumban. hello hello kimenete csökkentése replikációs állított too1 helyett hello alapértelmezett 3, mivel hello teljesítményteszt környezetével nem szükséges, hogy toomultiple csomópontján replikálja hello kimeneti adatok.

Hello mintát, minden egyes megfelelő tooone hello MapReduce programok hello bemutatása leírt három feladatok szükségesek:

1. Hello adatokat hello futtatásával rendezéshez létre **TeraGen** MapReduce feladatot.
2. Hello adatok rendezéséhez hello futtatásával **TeraSort** MapReduce feladatot.
3. Győződjön meg arról, hogy hello adatok rendelkezik megfelelően vannak rendezve hello futtatásával **TeraValidate** MapReduce feladatot.

**toosubmit hello feladatok**

* Hello eljárás a [Word-count - Java](#word-count-java), és használjon hello következő feladatdefiníciók:

    ```powershell
    $teragen = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teragen" `
                                -Arguments "-Dmapred.map.tasks=50", "100000000", "/example/data/10GB-sort-input"

    $terasort = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "terasort" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-input", "/example/data/10GB-sort-output"

    $teravalidate = New-AzureRmHDInsightMapReduceJobDefinition `
                                -JarFile "/example/jars/hadoop-mapreduce-examples.jar" `
                                -ClassName "teravalidate" `
                                -Arguments "-Dmapred.map.tasks=50", "-Dmapred.reduce.tasks=25", "/example/data/10GB-sort-output", "/example/data/10GB-sort-validate"
    ```

## <a name="next-steps"></a>Következő lépések
Ez a cikk és hello cikkek hello minták mindegyikén megtanulta, hogyan toorun hello minták mellékelt hello a HDInsight-fürtök Azure PowerShell használatával. A Pig, a Hive és a MapReduce és a HDInsight együttes használatával kapcsolatos oktatóanyagok tekintse meg a következő témakörök hello:

* [Hadoop használatának megkezdésében a Hive HDInsight tooanalyze mobil kézibeszélő használatban][hdinsight-get-started]
* [A Pig használata a HDInsight Hadoop][hdinsight-use-pig]
* [A Hive használata a hdinsight Hadoop][hdinsight-use-hive]
* [Küldje el a Hadoop-feladatokat a Hdinsightban][hdinsight-submit-jobs]
* [Az Azure HDInsight SDK-dokumentáció][hdinsight-sdk-documentation]

## <a name="appendix-a---hello-word-count-source-code"></a>A függelék – hello Word-count forráskód

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

## <a name="appendix-b---hello-word-count-streaming-source-code"></a>B függelék – hello szószámot adatfolyam-forráskód
hello MapReduce program hello csökkentése felület toocount hello dokumentum átvitt szavak száma hello cat.exe alkalmazás leképezési kapcsolat toostream hello szövegként hello konzolba és hello wc.exe alkalmazás használja. Hello leképező és a nyomáscsökkentő karakterek,--soronként, a hello szabványos bemeneti folyama (stdin) és olvasási toohello szabványos kimeneti folyam (stdout).

```csharp
// hello source code for hello cat.exe (Mapper).

using System;
using System.IO;

namespace cat
{
    class cat
    {
        static void Main(string[] args)
        {
            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            string line;
            char[] separators = { ' ', '\n'};
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split(separators);
                foreach (var word in words)
                {
                    Console.WriteLine("{0}\t1", word);
                }
            }
        }
    }
}
```

hello hello cat.cs fájl által használt leképező kód egy [StreamReader] [ streamreader] tooread hello karakterek hello bejövő adatfolyam toohello konzol, amely azután bejegyzést ír hello adatfolyam toohello szabványos kimeneti adatfolyam objektum a statikus hello [Console.WriteLine("a(z)] [ console-writeline] metódust.

```csharp
// hello source code for wc.exe (Reducer) is:

using System;
using System.IO;
using System.Linq;
using System.Collections;

namespace wc
{
    class wc
    {
        static void Main(string[] args)
        {
            string line;

            if (args.Length > 0)
            {
                Console.SetIn(new StreamReader(args[0]));
            }

            Hashtable wordCount = new Hashtable();
            while ((line = Console.ReadLine()) != null)
            {
                string[] words = line.Split('\t');

                string key = words[0];

                if (wordCount.ContainsKey(key) == true)
                {
                    int n = Convert.ToInt32(wordCount[key]);
                    wordCount[key] = Convert.ToString(n + 1);
                }
                else
                {
                    wordCount[key] = words[1];
                }
            }
            foreach (var key in wordCount.Keys)
            {
                Console.WriteLine("{0} {1}", key, wordCount[key]);
            }
        }
    }
}
```

hello hello wc.cs fájl által használt nyomáscsökkentő kód egy [StreamReader] [ streamreader] tooread karakterek adatfolyamból hello szabványos bemeneti, kimeneti hello cat.exe hozzárendelő már az objektum. Mivel hello karakterből állhat, és hello olvassa be [Console.WriteLine("a(z)] [ console-writeline] módszer, megszámlálja hello szavak alapján szóközök és a sor végén karakterek hello végén lévő minden szó. Ezután ír a hello teljes toohello szabványos kimeneti adatfolyam hello [Console.WriteLine("a(z)] [ console-writeline] metódust.

## <a name="appendix-c---hello-pi-estimator-source-code"></a>C függelék – hello Pi négyzetgyökének forráskód
hello pi négyzetgyökének hello hozzárendelést és nyomáscsökkentő funkciók tartalmazó Java-kódot az alábbi hálózatfelügyeleti érhető el. hello leképező program véletlenszerű helyezni egy egységre négyszög pontok megadott számú létrehozza, és majd megjeleníti az adott hello körben pontok hello számát. hello nyomáscsökkentő program által hello mappers megszámlált pontok összesít és hello képlet 4R, ahol R az belül hello kör toohello pontok számát belüli négyzetes hello számlált pontok hello száma hello aránya a pi értékét hello majd becslése.

```java
/**
* Licensed toohello Apache Software Foundation (ASF) under one
* or more contributor license agreements. See hello NOTICE file
* distributed with this work for additional information
* regarding copyright ownership. hello ASF licenses this file
* tooyou under hello Apache License, Version 2.0 (the
* "License"); you may not use this file except in compliance
* with hello License. You may obtain a copy of hello License at
*
* http://www.apache.org/licenses/LICENSE-2.0
*
* Unless required by applicable law or agreed tooin writing, software
* distributed under hello License is distributed on an "AS IS" BASIS,
* WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or     implied.
* See hello License for hello specific language governing permissions and
* limitations under hello License.
*/

package org.apache.hadoop.examples;

import java.io.IOException;
import java.math.BigDecimal;
import java.util.Iterator;

import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.BooleanWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Writable;
import org.apache.hadoop.io.WritableComparable;
import org.apache.hadoop.io.SequenceFile.CompressionType;
import org.apache.hadoop.mapred.FileInputFormat;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.MapReduceBase;
import org.apache.hadoop.mapred.Mapper;
import org.apache.hadoop.mapred.OutputCollector;
import org.apache.hadoop.mapred.Reducer;
import org.apache.hadoop.mapred.Reporter;
import org.apache.hadoop.mapred.SequenceFileInputFormat;
import org.apache.hadoop.mapred.SequenceFileOutputFormat;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

//A Map-reduce program tooestimate hello value of Pi
//using quasi-Monte Carlo method.
//
//Mapper:
//Generate points in a unit square
//and then count points inside/outside of hello inscribed circle of hello square.
//
//Reducer:
//Accumulate points inside/outside results from hello mappers.
//Let numTotal = numInside + numOutside.
//hello fraction numInside/numTotal is a rational approximation of
//hello value (Area of hello circle)/(Area of hello square),
//where hello area of hello inscribed circle is Pi/4
//and hello area of unit square is 1.
//Then, Pi is estimated value toobe 4(numInside/numTotal).
//

public class PiEstimator extends Configured implements Tool {
//tmp directory for input/output
static private final Path TMP_DIR = new Path(
PiEstimator.class.getSimpleName() + "_TMP_3_141592654");

//2-dimensional Halton sequence {H(i)},
//where H(i) is a 2-dimensional point and i >= 1 is hello index.
//Halton sequence is used toogenerate sample points for Pi estimation.
private static class HaltonSequence {
// Bases
static final int[] P = {2, 3};
//Maximum number of digits allowed
static final int[] K = {63, 40};

private long index;
private double[] x;
private double[][] q;
private int[][] d;

//Initialize tooH(startindex),
//so hello sequence begins with H(startindex+1).
HaltonSequence(long startindex) {
index = startindex;
x = new double[K.length];
q = new double[K.length][];
d = new int[K.length][];
for(int i = 0; i < K.length; i++) {
q[i] = new double[K[i]];
d[i] = new int[K[i]];
}

for(int i = 0; i < K.length; i++) {
long k = index;
x[i] = 0;

for(int j = 0; j < K[i]; j++) {
q[i][j] = (j == 0? 1.0: q[i][j-1])/P[i];
d[i][j] = (int)(k % P[i]);
k = (k - d[i][j])/P[i];
x[i] += d[i][j] * q[i][j];
}
}
}

//Compute next point.
//Assume hello current point is H(index).
//Compute H(index+1).
//@return a 2-dimensional point with coordinates in [0,1)^2
double[] nextPoint() {
index++;
for(int i = 0; i < K.length; i++) {
for(int j = 0; j < K[i]; j++) {
d[i][j]++;
x[i] += q[i][j];
if (d[i][j] < P[i]) {
break;
}
d[i][j] = 0;
x[i] -= (j == 0? 1.0: q[i][j-1]);
}
}
return x;
}
}

//Mapper class for Pi estimation.
//Generate points in a unit square and then
//count points inside/outside of hello inscribed circle of hello square.
public static class PiMapper extends MapReduceBase
implements Mapper<LongWritable, LongWritable, BooleanWritable, LongWritable> {

//Map method.
//@param offset samples starting from hello (offset+1)th sample.
//@param size hello number of samples for this map
//@param out output {ture->numInside, false->numOutside}
//@param reporter
public void map(LongWritable offset,
LongWritable size,
OutputCollector<BooleanWritable, LongWritable> out,
Reporter reporter) throws IOException {

final HaltonSequence haltonsequence = new HaltonSequence(offset.get());
long numInside = 0L;
long numOutside = 0L;

for(long i = 0; i < size.get(); ) {
//generate points in a unit square
final double[] point = haltonsequence.nextPoint();

//count points inside/outside of hello inscribed circle of hello square
final double x = point[0] - 0.5;
final double y = point[1] - 0.5;
if (x*x + y*y > 0.25) {
numOutside++;
} else {
numInside++;
}

//report status
i++;
if (i % 1000 == 0) {
reporter.setStatus("Generated " + i + " samples.");
}
}

//output map results
out.collect(new BooleanWritable(true), new LongWritable(numInside));
out.collect(new BooleanWritable(false), new LongWritable(numOutside));
}
}

//Reducer class for Pi estimation.
//Accumulate points inside/outside results from hello mappers.
public static class PiReducer extends MapReduceBase
implements Reducer<BooleanWritable, LongWritable, WritableComparable<?>, Writable> {

private long numInside = 0;
private long numOutside = 0;
private JobConf conf; //configuration for accessing hello file system

//Store job configuration.
@Override
public void configure(JobConf job) {
conf = job;
}

// Accumulate number of points inside/outside results from hello mappers.
// @param isInside Is hello points inside?
// @param values An iterator tooa list of point counts
// @param output dummy, not used here.
// @param reporter

public void reduce(BooleanWritable isInside,
Iterator<LongWritable> values,
OutputCollector<WritableComparable<?>, Writable> output,
Reporter reporter) throws IOException {
if (isInside.get()) {
for(; values.hasNext(); numInside += values.next().get());
} else {
for(; values.hasNext(); numOutside += values.next().get());
}
}

//Reduce task done, write output tooa file.
@Override
public void close() throws IOException {
//write output tooa file
Path outDir = new Path(TMP_DIR, "out");
Path outFile = new Path(outDir, "reduce-out");
FileSystem fileSys = FileSystem.get(conf);
SequenceFile.Writer writer = SequenceFile.createWriter(fileSys, conf,
outFile, LongWritable.class, LongWritable.class,
CompressionType.NONE);
writer.append(new LongWritable(numInside), new LongWritable(numOutside));
writer.close();
}
}

//Run a map/reduce job for estimating Pi.
//@return hello estimated value of Pi.
public static BigDecimal estimate(int numMaps, long numPoints, JobConf jobConf
)
throws IOException {
//setup job conf
jobConf.setJobName(PiEstimator.class.getSimpleName());

jobConf.setInputFormat(SequenceFileInputFormat.class);

jobConf.setOutputKeyClass(BooleanWritable.class);
jobConf.setOutputValueClass(LongWritable.class);
jobConf.setOutputFormat(SequenceFileOutputFormat.class);

jobConf.setMapperClass(PiMapper.class);
jobConf.setNumMapTasks(numMaps);

jobConf.setReducerClass(PiReducer.class);
jobConf.setNumReduceTasks(1);

// turn off speculative execution, because DFS doesn't handle
// multiple writers toohello same file.
jobConf.setSpeculativeExecution(false);

//setup input/output directories
final Path inDir = new Path(TMP_DIR, "in");
final Path outDir = new Path(TMP_DIR, "out");
FileInputFormat.setInputPaths(jobConf, inDir);
FileOutputFormat.setOutputPath(jobConf, outDir);

final FileSystem fs = FileSystem.get(jobConf);
if (fs.exists(TMP_DIR)) {
throw new IOException("Tmp directory " + fs.makeQualified(TMP_DIR)
+ " already exists. Remove it first.");
}
if (!fs.mkdirs(inDir)) {
throw new IOException("Cannot create input directory " + inDir);
}

//generate an input file for each map task
try {
for(int i=0; i < numMaps; ++i) {
final Path file = new Path(inDir, "part"+i);
final LongWritable offset = new LongWritable(i * numPoints);
final LongWritable size = new LongWritable(numPoints);
final SequenceFile.Writer writer = SequenceFile.createWriter(
fs, jobConf, file,
LongWritable.class, LongWritable.class, CompressionType.NONE);
try {
writer.append(offset, size);
} finally {
writer.close();
}
System.out.println("Wrote input for Map #"+i);
}

//start a map/reduce job
System.out.println("Starting Job");
final long startTime = System.currentTimeMillis();
JobClient.runJob(jobConf);
final double duration = (System.currentTimeMillis() - startTime)/1000.0;
System.out.println("Job Finished in " + duration + " seconds");

//read outputs
Path inFile = new Path(outDir, "reduce-out");
LongWritable numInside = new LongWritable();
LongWritable numOutside = new LongWritable();
SequenceFile.Reader reader = new SequenceFile.Reader(fs, inFile, jobConf);
try {
reader.next(numInside, numOutside);
} finally {
reader.close();
}

//compute estimated value
return BigDecimal.valueOf(4).setScale(20)
.multiply(BigDecimal.valueOf(numInside.get()))
.divide(BigDecimal.valueOf(numMaps))
.divide(BigDecimal.valueOf(numPoints));
} finally {
fs.delete(TMP_DIR, true);
}
}

//Parse arguments and then runs a map/reduce job.
//Print output in standard out.
//@return a non-zero if there is an error. Otherwise, return 0.
public int run(String[] args) throws Exception {
if (args.length != 2) {
System.err.println("Usage: "+getClass().getName()+" <nMaps> <nSamples>");
ToolRunner.printGenericCommandUsage(System.err);
return -1;
}

final int nMaps = Integer.parseInt(args[0]);
final long nSamples = Long.parseLong(args[1]);

System.out.println("Number of Maps = " + nMaps);
System.out.println("Samples per Map = " + nSamples);

final JobConf jobConf = new JobConf(getConf(), getClass());
System.out.println("Estimated value of Pi is "
+ estimate(nMaps, nSamples, jobConf));
return 0;
}

//main method for running it as a stand alone command.
public static void main(String[] argv) throws Exception {
System.exit(ToolRunner.run(null, new PiEstimator(), argv));
}
}
```

## <a name="appendix-d---hello-10gb-graysort-source-code"></a>D – hello 10 GB-os graysort forráskód függelék
hello TeraSort MapReduce program hello kódját az ebben a szakaszban ellenőrző áll rendelkezésre.

```java
/**
    * Licensed toohello Apache Software Foundation (ASF) under one
    * or more contributor license agreements.  See hello NOTICE file
    * distributed with this work for additional information
    * regarding copyright ownership.  hello ASF licenses this file
    * tooyou under hello Apache License, Version 2.0 (the
    * "License"); you may not use this file except in compliance
    * with hello License.  You may obtain a copy of hello License at
    *
    *     http://www.apache.org/licenses/LICENSE-2.0
    *
    * Unless required by applicable law or agreed tooin writing, software
    * distributed under hello License is distributed on an "AS IS" BASIS,
    * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    * See hello License for hello specific language governing permissions and
    * limitations under hello License.
    */

package org.apache.hadoop.examples.terasort;

import java.io.IOException;
import java.io.PrintStream;
import java.net.URI;
import java.util.ArrayList;
import java.util.List;

import org.apache.commons.logging.Log;
import org.apache.commons.logging.LogFactory;
import org.apache.hadoop.conf.Configured;
import org.apache.hadoop.filecache.DistributedCache;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.NullWritable;
import org.apache.hadoop.io.SequenceFile;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapred.FileOutputFormat;
import org.apache.hadoop.mapred.JobClient;
import org.apache.hadoop.mapred.JobConf;
import org.apache.hadoop.mapred.Partitioner;
import org.apache.hadoop.util.Tool;
import org.apache.hadoop.util.ToolRunner;

/**
    * Generates hello sampled split points, launches hello job,
    * and waits for it toofinish.
    * <p>
    * toorun hello program:
    * <b>bin/hadoop jar hadoop-examples-*.jar terasort in-dir out-dir</b>
    */

public class TeraSort extends Configured implements Tool {
    private static final Log LOG = LogFactory.getLog(TeraSort.class);

    /**
    * A partitioner that splits text keys into roughly equal
    * partitions in a global sorted order.
    */

    static class TotalOrderPartitioner implements Partitioner<Text,Text>{
    private TrieNode trie;
    private Text[] splitPoints;

    /**
        * A generic trie node
        */
    static abstract class TrieNode {
        private int level;
        TrieNode(int level) {
        this.level = level;
        }
        abstract int findPartition(Text key);
        abstract void print(PrintStream strm) throws IOException;
        int getLevel() {
        return level;
        }
    }

    /**
        * An inner trie node that contains 256 children based on hello next
        * character.
        */
    static class InnerTrieNode extends TrieNode {
        private TrieNode[] child = new TrieNode[256];

        InnerTrieNode(int level) {
        super(level);
        }
        int findPartition(Text key) {
        int level = getLevel();
        if (key.getLength() <= level) {
            return child[0].findPartition(key);
        }
        return child[key.getBytes()[level]].findPartition(key);
        }
        void setChild(int idx, TrieNode child) {
        this.child[idx] = child;
        }
        void print(PrintStream strm) throws IOException {
        for(int ch=0; ch < 255; ++ch) {
            for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
            }
            strm.print(ch);
            strm.println(" ->");
            if (child[ch] != null) {
            child[ch].print(strm);
            }
        }
        }
    }

    /**
        * A leaf trie node that does string compares toofigure out where hello given
        * key belongs between lower..upper.
        */
    static class LeafTrieNode extends TrieNode {
        int lower;
        int upper;
        Text[] splitPoints;
        LeafTrieNode(int level, Text[] splitPoints, int lower, int upper) {
        super(level);
        this.splitPoints = splitPoints;
        this.lower = lower;
        this.upper = upper;
        }
        int findPartition(Text key) {
        for(int i=lower; i<upper; ++i) {
            if (splitPoints[i].compareTo(key) >= 0) {
            return i;
            }
        }
        return upper;
        }
        void print(PrintStream strm) throws IOException {
        for(int i = 0; i < 2*getLevel(); ++i) {
            strm.print(' ');
        }
        strm.print(lower);
        strm.print(", ");
        strm.println(upper);
        }
    }

    /**
        * Read hello cut points from hello given sequence file.
        * @param fs hello file system
        * @param p hello path tooread
        * @param job hello job config
        * @return hello strings toosplit hello partitions on
        * @throws IOException
        */
    private static Text[] readPartitions(FileSystem fs, Path p,
                                            JobConf job) throws IOException {
        SequenceFile.Reader reader = new SequenceFile.Reader(fs, p, job);
        List<Text> parts = new ArrayList<Text>();
        Text key = new Text();
        NullWritable value = NullWritable.get();
        while (reader.next(key, value)) {
        parts.add(key);
        key = new Text();
        }
        reader.close();
        return parts.toArray(new Text[parts.size()]);
    }

    /**
        * Given a sorted set of cut points, build a trie that will find hello correct
        * partition quickly.
        * @param splits hello list of cut points
        * @param lower hello lower bound of partitions 0..numPartitions-1
        * @param upper hello upper bound of partitions 0..numPartitions-1
        * @param prefix hello prefix that we have already checked against
        * @param maxDepth hello maximum depth we will build a trie for
        * @return hello trie node that will divide hello splits correctly
        */
    private static TrieNode buildTrie(Text[] splits, int lower, int upper,
                                        Text prefix, int maxDepth) {
        int depth = prefix.getLength();
        if (depth >= maxDepth || lower == upper) {
        return new LeafTrieNode(depth, splits, lower, upper);
        }
        InnerTrieNode result = new InnerTrieNode(depth);
        Text trial = new Text(prefix);
        // append an extra byte on toohello prefix
        trial.append(new byte[1], 0, 1);
        int currentBound = lower;
        for(int ch = 0; ch < 255; ++ch) {
        trial.getBytes()[depth] = (byte) (ch + 1);
        lower = currentBound;
        while (currentBound < upper) {
            if (splits[currentBound].compareTo(trial) >= 0) {
            break;
            }
            currentBound += 1;
        }
        trial.getBytes()[depth] = (byte) ch;
        result.child[ch] = buildTrie(splits, lower, currentBound, trial,
                                        maxDepth);
        }
        // pick up hello rest
        trial.getBytes()[depth] = 127;
        result.child[255] = buildTrie(splits, currentBound, upper, trial,
                                    maxDepth);
        return result;
    }

    public void configure(JobConf job) {
        try {
        FileSystem fs = FileSystem.getLocal(job);
        Path partFile = new Path(TeraInputFormat.PARTITION_FILENAME);
        splitPoints = readPartitions(fs, partFile, job);
        trie = buildTrie(splitPoints, 0, splitPoints.length, new Text(), 2);
        } catch (IOException ie) {
        throw new IllegalArgumentException("can't read paritions file", ie);
        }
    }

    public TotalOrderPartitioner() {
    }

    public int getPartition(Text key, Text value, int numPartitions) {
        return trie.findPartition(key);
    }

    }

    public int run(String[] args) throws Exception {
    LOG.info("starting");
    JobConf job = (JobConf) getConf();
    Path inputDir = new Path(args[0]);
    inputDir = inputDir.makeQualified(inputDir.getFileSystem(job));
    Path partitionFile = new Path(inputDir, TeraInputFormat.PARTITION_FILENAME);
    URI partitionUri = new URI(partitionFile.toString() +
                                "#" + TeraInputFormat.PARTITION_FILENAME);
    TeraInputFormat.setInputPaths(job, new Path(args[0]));
    FileOutputFormat.setOutputPath(job, new Path(args[1]));
    job.setJobName("TeraSort");
    job.setJarByClass(TeraSort.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(Text.class);
    job.setInputFormat(TeraInputFormat.class);
    job.setOutputFormat(TeraOutputFormat.class);
    job.setPartitionerClass(TotalOrderPartitioner.class);
    TeraInputFormat.writePartitionFile(job, partitionFile);
    DistributedCache.addCacheFile(partitionUri, job);
    DistributedCache.createSymlink(job);
    job.setInt("dfs.replication", 1);
    TeraOutputFormat.setFinalSync(job, true);
    JobClient.runJob(job);
    LOG.info("done");
    return 0;
    }

    /**
    * @param args
    */

    public static void main(String[] args) throws Exception {
    int res = ToolRunner.run(new JobConf(), new TeraSort(), args);
    System.exit(res);
    }
}
```

[hdinsight-sdk-documentation]: https://msdn.microsoft.com/library/azure/dn479185.aspx

[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-introduction]: hdinsight-hadoop-introduction.md

[powershell-install-configure]: /powershell/azureps-cmdlets-docs

[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[hdinsight-samples]: hdinsight-run-samples.md
[hdinsight-sample-10gb-graysort]: #hdinsight-sample-10gb-graysort
[hdinsight-sample-csharp-streaming]: #hdinsight-sample-csharp-streaming
[hdinsight-sample-pi-estimator]: #hdinsight-sample-pi-estimator
[hdinsight-sample-wordcount]: #hdinsight-sample-wordcount

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[streamreader]: http://msdn.microsoft.com/library/system.io.streamreader.aspx
[console-writeline]: http://msdn.microsoft.com/library/system.console.writeline
[stdin-stdout-stderr]: https://msdn.microsoft.com/library/3x292kth.aspx
