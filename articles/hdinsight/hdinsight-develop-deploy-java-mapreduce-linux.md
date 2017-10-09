---
title: Java-MapReduce a Hadoop - Azure HDInsight aaaCreate |} Microsoft Docs
description: "Megtudhatja, hogyan toouse Apache Maven Java-alapú toocreate MapReduce alkalmazást, majd futtassa a Hadoop on Azure HDInsight."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: Blackmist
documentationcenter: 
tags: azure-portal
ms.assetid: 9ee6384c-cb61-4087-8273-fb53fa27c1c3
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 903a57a482395f7da79002188399a4d6288ff0af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a><span data-ttu-id="81853-103">A hdinsight Hadoop Java MapReduce programok fejlesztése</span><span class="sxs-lookup"><span data-stu-id="81853-103">Develop Java MapReduce programs for Hadoop on HDInsight</span></span>

<span data-ttu-id="81853-104">Megtudhatja, hogyan toouse Apache Maven Java-alapú toocreate MapReduce alkalmazást, majd futtassa a Hadoop on Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="81853-104">Learn how toouse Apache Maven toocreate a Java-based MapReduce application, then run it with Hadoop on Azure HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="81853-105">Ebben a példában a HDInsight 3.6 legutóbb teszteltük.</span><span class="sxs-lookup"><span data-stu-id="81853-105">This example was most recently tested on HDInsight 3.6.</span></span>

## <span data-ttu-id="81853-106"><a name="prerequisites"></a>Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="81853-106"><a name="prerequisites"></a>Prerequisites</span></span>

* <span data-ttu-id="81853-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 vagy újabb (vagy egy azzal egyenértékű, például OpenJDK).</span><span class="sxs-lookup"><span data-stu-id="81853-107">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK).</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="81853-108">HDInsight-verziókról 3.4 és korábbi Java 7 használja.</span><span class="sxs-lookup"><span data-stu-id="81853-108">HDInsight versions 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="81853-109">HDInsight 3.5-ös és nagyobb Java 8 használja.</span><span class="sxs-lookup"><span data-stu-id="81853-109">HDInsight 3.5 and greater uses Java 8.</span></span>

* [<span data-ttu-id="81853-110">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="81853-110">Apache Maven</span></span>](http://maven.apache.org/)

## <a name="configure-development-environment"></a><span data-ttu-id="81853-111">A fejlesztési környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="81853-111">Configure development environment</span></span>

<span data-ttu-id="81853-112">hello következő környezeti változó lehet beállítani a Java és hello JDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="81853-112">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="81853-113">Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="81853-113">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="81853-114">`JAVA_HOME`-hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="81853-114">`JAVA_HOME` - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="81853-115">Például az OS X, Unix vagy Linux rendszeren nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="81853-115">For example, on an OS X, Unix or Linux system, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="81853-116">A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="81853-116">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="81853-117">`PATH`-a következő elérési utak hello tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="81853-117">`PATH` - should contain hello following paths:</span></span>
  
  * <span data-ttu-id="81853-118">`JAVA_HOME`(vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="81853-118">`JAVA_HOME` (or hello equivalent path)</span></span>

  * <span data-ttu-id="81853-119">`JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="81853-119">`JAVA_HOME\bin` (or hello equivalent path)</span></span>

  * <span data-ttu-id="81853-120">hello mappát, ahová a Maven telepítve van</span><span class="sxs-lookup"><span data-stu-id="81853-120">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="81853-121">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="81853-121">Create a Maven project</span></span>

1. <span data-ttu-id="81853-122">Egy terminál-munkamenetet, vagy a parancssori a fejlesztési környezetben módosítsa a könyvtárakat toohello helyre toostore ebben a projektben.</span><span class="sxs-lookup"><span data-stu-id="81853-122">From a terminal session, or command line in your development environment, change directories toohello location you want toostore this project.</span></span>

2. <span data-ttu-id="81853-123">Használjon hello `mvn` parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.</span><span class="sxs-lookup"><span data-stu-id="81853-123">Use hello `mvn` command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > <span data-ttu-id="81853-124">Ha a PowerShell használata esetén hello közé kell `-D` az idézőjelek közé foglalt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="81853-124">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="81853-125">Ez a parancs létrehoz egy könyvtárat hello által megadott hello nevű `artifactID` paraméter (**wordcountjava** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="81853-125">This command creates a directory with hello name specified by hello `artifactID` parameter (**wordcountjava** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="81853-126">`pom.xml`-hello [projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , amely ismerteti, és konfigurációs részletek használt toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="81853-126">`pom.xml` - hello [Project Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) that contains information and configuration details used toobuild hello project.</span></span>

   * <span data-ttu-id="81853-127">`src`-hello alkalmazást tartalmazó hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="81853-127">`src` - hello directory that contains hello application.</span></span>

3. <span data-ttu-id="81853-128">Törölje a hello `src/test/java/org/apache/hadoop/examples/apptest.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="81853-128">Delete hello `src/test/java/org/apache/hadoop/examples/apptest.java` file.</span></span> <span data-ttu-id="81853-129">Ebben a példában nem szolgál.</span><span class="sxs-lookup"><span data-stu-id="81853-129">It is not used in this example.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="81853-130">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="81853-130">Add dependencies</span></span>

1. <span data-ttu-id="81853-131">Hello szerkesztése `pom.xml` fájlt, és adja hozzá a következő szöveget hello hello `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="81853-131">Edit hello `pom.xml` file and add hello following text inside hello `<dependencies>` section:</span></span>
   
   ```xml
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-examples</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-mapreduce-client-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.7.3</version>
        <scope>provided</scope>
    </dependency>
   ```

    <span data-ttu-id="81853-132">Ez határozza meg a szükséges kódtárak (belül felsorolt &lt;artifactid szakaszát\>) megadott verzióval (belül felsorolt &lt;verzió\>).</span><span class="sxs-lookup"><span data-stu-id="81853-132">This defines required libraries (listed within &lt;artifactId\>) with a specific version (listed within &lt;version\>).</span></span> <span data-ttu-id="81853-133">Fordítási időben a függőségek hello alapértelmezett Maven tárházból lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="81853-133">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="81853-134">Használhatja a hello [Maven tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) további tooview.</span><span class="sxs-lookup"><span data-stu-id="81853-134">You can use hello [Maven repository search](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) tooview more.</span></span>
   
    <span data-ttu-id="81853-135">Hello `<scope>provided</scope>` közli Maven, hogy az ezen függőségekhez nem csomagolható hello alkalmazással hello HDInsight-fürt futásidőben meghatározott.</span><span class="sxs-lookup"><span data-stu-id="81853-135">hello `<scope>provided</scope>` tells Maven that these dependencies should not be packaged with hello application, as they are provided by hello HDInsight cluster at run-time.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="81853-136">hello használt verziójának meg kell felelnie hello verziója található meg a fürt Hadoop.</span><span class="sxs-lookup"><span data-stu-id="81853-136">hello version used should match hello version of Hadoop present on your cluster.</span></span> <span data-ttu-id="81853-137">-Verziókban további információkért lásd: hello [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="81853-137">For more information on versions, see hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

2. <span data-ttu-id="81853-138">Adja hozzá a következő toohello hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="81853-138">Add hello following toohello `pom.xml` file.</span></span> <span data-ttu-id="81853-139">Ez a szöveg hello belül kell `<project>...</project>` címkék hello fájlban, például közötti `</dependencies>` és `</project>`.</span><span class="sxs-lookup"><span data-stu-id="81853-139">This text must be inside hello `<project>...</project>` tags in hello file; for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
    <build>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
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
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.6.1</version>
            <configuration>
            <source>1.8</source>
            <target>1.8</target>
            </configuration>
        </plugin>
        </plugins>
    </build>
   ```

    <span data-ttu-id="81853-140">hello első beépülő modul konfigurálása hello [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/), mely használt toobuild (más néven a fatjar), egy uberjar hello alkalmazáshoz szükséges függőségek tartalmazó van.</span><span class="sxs-lookup"><span data-stu-id="81853-140">hello first plugin configures hello [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/), which is used toobuild an uberjar (sometimes called a fatjar), which contains dependencies required by hello application.</span></span> <span data-ttu-id="81853-141">Megakadályozza a párhuzamos licencek hello jar csomagon belül, amely az egyes rendszerek problémákat okozhat.</span><span class="sxs-lookup"><span data-stu-id="81853-141">It also prevents duplication of licenses within hello jar package, which can cause problems on some systems.</span></span>

    <span data-ttu-id="81853-142">hello második beépülő modul konfigurálása hello cél Java-verziót.</span><span class="sxs-lookup"><span data-stu-id="81853-142">hello second plugin configures hello target Java version.</span></span>

    > [!NOTE]
    > <span data-ttu-id="81853-143">HDInsight 3.4-es és korábbi használat Java 7.</span><span class="sxs-lookup"><span data-stu-id="81853-143">HDInsight 3.4 and earlier use Java 7.</span></span> <span data-ttu-id="81853-144">HDInsight 3.5-ös és nagyobb Java 8 használja.</span><span class="sxs-lookup"><span data-stu-id="81853-144">HDInsight 3.5 and greater uses Java 8.</span></span>

3. <span data-ttu-id="81853-145">Mentse a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="81853-145">Save hello `pom.xml` file.</span></span>

## <a name="create-hello-mapreduce-application"></a><span data-ttu-id="81853-146">Hello MapReduce-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="81853-146">Create hello MapReduce application</span></span>

1. <span data-ttu-id="81853-147">Nyissa meg toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` könyvtár, és nevezze át hello `App.java` fájl túl`WordCount.java`.</span><span class="sxs-lookup"><span data-stu-id="81853-147">Go toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` directory and rename hello `App.java` file too`WordCount.java`.</span></span>

2. <span data-ttu-id="81853-148">Nyissa meg hello `WordCount.java` fájlt egy szövegszerkesztőben, és cserélje ki a következő szöveg hello hello tartalmát:</span><span class="sxs-lookup"><span data-stu-id="81853-148">Open hello `WordCount.java` file in a text editor and replace hello contents with hello following text:</span></span>
   
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
   
    <span data-ttu-id="81853-149">Értesítés hello csomag neve `org.apache.hadoop.examples` hello osztály neve pedig `WordCount`.</span><span class="sxs-lookup"><span data-stu-id="81853-149">Notice hello package name is `org.apache.hadoop.examples` and hello class name is `WordCount`.</span></span> <span data-ttu-id="81853-150">Amikor hello MapReduce feladatot használhatja ezeket a neveket.</span><span class="sxs-lookup"><span data-stu-id="81853-150">You use these names when you submit hello MapReduce job.</span></span>

3. <span data-ttu-id="81853-151">Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="81853-151">Save hello file.</span></span>

## <a name="build-hello-application"></a><span data-ttu-id="81853-152">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="81853-152">Build hello application</span></span>

1. <span data-ttu-id="81853-153">Módosítsa a toohello `wordcountjava` könyvtárat, ha nem már létezik.</span><span class="sxs-lookup"><span data-stu-id="81853-153">Change toohello `wordcountjava` directory, if you are not already there.</span></span>

2. <span data-ttu-id="81853-154">A következő parancs toobuild a JAR-fájlt tartalmazó hello alkalmazás hello használata:</span><span class="sxs-lookup"><span data-stu-id="81853-154">Use hello following command toobuild a JAR file containing hello application:</span></span>

   ```
   mvn clean package
   ```

    <span data-ttu-id="81853-155">Ez a parancs törli az előző build összetevőihez, töltse le a bármely függőségek, amelyek nem már telepítve van, és ezután buildek és csomag hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="81853-155">This command cleans any previous build artifacts, downloads any dependencies that have not already been installed, and then builds and package hello application.</span></span>

3. <span data-ttu-id="81853-156">Miután hello parancs futtatásának befejeztével hello `wordcountjava/target` directory nevű fájlt tartalmaz `wordcountjava-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="81853-156">Once hello command finishes, hello `wordcountjava/target` directory contains a file named `wordcountjava-1.0-SNAPSHOT.jar`.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="81853-157">Hello `wordcountjava-1.0-SNAPSHOT.jar` fájl egy uberjar, nem csak a hello WordCount feladatot tartalmazó, de tartalomfüggőségeket, ezt a feladatot hello megköveteli azt is, futási időben.</span><span class="sxs-lookup"><span data-stu-id="81853-157">hello `wordcountjava-1.0-SNAPSHOT.jar` file is an uberjar, which contains not only hello WordCount job, but also dependencies that hello job requires at runtime.</span></span>

## <span data-ttu-id="81853-158"><a id="upload"></a>Hello jar feltöltése</span><span class="sxs-lookup"><span data-stu-id="81853-158"><a id="upload"></a>Upload hello jar</span></span>

<span data-ttu-id="81853-159">A következő parancs tooupload hello jar fájl toohello HDInsight headnode hello használata:</span><span class="sxs-lookup"><span data-stu-id="81853-159">Use hello following command tooupload hello jar file toohello HDInsight headnode:</span></span>

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

<span data-ttu-id="81853-160">Ez a parancs hello helyi rendszer toohello átjárócsomópont hello fájlokat másolja át.</span><span class="sxs-lookup"><span data-stu-id="81853-160">This command copies hello files from hello local system toohello head node.</span></span> <span data-ttu-id="81853-161">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="81853-161">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <span data-ttu-id="81853-162"><a name="run"></a>A Hadoop hello MapReduce feladat futtatása</span><span class="sxs-lookup"><span data-stu-id="81853-162"><a name="run"></a>Run hello MapReduce job on Hadoop</span></span>

1. <span data-ttu-id="81853-163">Csatlakozzon az SSH használatával tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="81853-163">Connect tooHDInsight using SSH.</span></span> <span data-ttu-id="81853-164">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="81853-164">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

2. <span data-ttu-id="81853-165">Hello SSH-munkamenetet használja a következő parancs toorun hello MapReduce alkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="81853-165">From hello SSH session, use hello following command toorun hello MapReduce application:</span></span>
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    <span data-ttu-id="81853-166">Hello WordCount MapReduce alkalmazás indítja el.</span><span class="sxs-lookup"><span data-stu-id="81853-166">This command starts hello WordCount MapReduce application.</span></span> <span data-ttu-id="81853-167">hello bemeneti fájl `/example/data/gutenberg/davinci.txt`, és a kimeneti könyvtár hello `/example/data/wordcountout`.</span><span class="sxs-lookup"><span data-stu-id="81853-167">hello input file is `/example/data/gutenberg/davinci.txt`, and hello output directory is `/example/data/wordcountout`.</span></span> <span data-ttu-id="81853-168">Hello bemeneti fájl és a kimeneti tárolt toohello alapértelmezett hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="81853-168">Both hello input file and output are stored toohello default storage for hello cluster.</span></span>

3. <span data-ttu-id="81853-169">Amikor hello feladat befejeződik, használja a hello parancs tooview hello eredménye a következő:</span><span class="sxs-lookup"><span data-stu-id="81853-169">Once hello job completes, use hello following command tooview hello results:</span></span>
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    <span data-ttu-id="81853-170">Szavak és számát, a szöveg a következő értékek hasonló toohello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="81853-170">You should receive a list of words and counts, with values similar toohello following text:</span></span>
   
        zeal    1
        zelus   1
        zenith  2

## <span data-ttu-id="81853-171"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="81853-171"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="81853-172">Ebből a dokumentumból megtanulta, hogyan toodevelop Java MapReduce feladatot.</span><span class="sxs-lookup"><span data-stu-id="81853-172">In this document, you have learned how toodevelop a Java MapReduce job.</span></span> <span data-ttu-id="81853-173">Tekintse meg a következő dokumentumok a hdinsight eszközzel más módon toowork hello.</span><span class="sxs-lookup"><span data-stu-id="81853-173">See hello following documents for other ways toowork with HDInsight.</span></span>

* <span data-ttu-id="81853-174">[A Hive használata a HDInsightban][hdinsight-use-hive]</span><span class="sxs-lookup"><span data-stu-id="81853-174">[Use Hive with HDInsight][hdinsight-use-hive]</span></span>
* <span data-ttu-id="81853-175">[A Pig használata a HDInsightban][hdinsight-use-pig]</span><span class="sxs-lookup"><span data-stu-id="81853-175">[Use Pig with HDInsight][hdinsight-use-pig]</span></span>
* [<span data-ttu-id="81853-176">Use MapReduce with HDInsight</span><span class="sxs-lookup"><span data-stu-id="81853-176">Use MapReduce with HDInsight</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="81853-177">További információkért lásd még: hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).</span><span class="sxs-lookup"><span data-stu-id="81853-177">For more information, see also hello [Java Developer Center](https://azure.microsoft.com/develop/java/).</span></span>

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md

[powershell-PSCredential]: http://social.technet.microsoft.com/wiki/contents/articles/4546.working-with-passwords-secure-strings-and-credentials-in-windows-powershell.aspx

