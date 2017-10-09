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
# <a name="develop-java-mapreduce-programs-for-hadoop-on-hdinsight"></a>A hdinsight Hadoop Java MapReduce programok fejlesztése

Megtudhatja, hogyan toouse Apache Maven Java-alapú toocreate MapReduce alkalmazást, majd futtassa a Hadoop on Azure HDInsight.

> [!NOTE]
> Ebben a példában a HDInsight 3.6 legutóbb teszteltük.

## <a name="prerequisites"></a>Előfeltételek

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 vagy újabb (vagy egy azzal egyenértékű, például OpenJDK).
    
    > [!NOTE]
    > HDInsight-verziókról 3.4 és korábbi Java 7 használja. HDInsight 3.5-ös és nagyobb Java 8 használja.

* [Apache Maven](http://maven.apache.org/)

## <a name="configure-development-environment"></a>A fejlesztési környezet konfigurálása

hello következő környezeti változó lehet beállítani a Java és hello JDK telepítésekor. Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.

* `JAVA_HOME`-hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia. Például az OS X, Unix vagy Linux rendszeren nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`. A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`

* `PATH`-a következő elérési utak hello tartalmaznia kell:
  
  * `JAVA_HOME`(vagy ezzel egyenértékű elérési hello)

  * `JAVA_HOME\bin`(vagy ezzel egyenértékű elérési hello)

  * hello mappát, ahová a Maven telepítve van

## <a name="create-a-maven-project"></a>Maven-projekt létrehozása

1. Egy terminál-munkamenetet, vagy a parancssori a fejlesztési környezetben módosítsa a könyvtárakat toohello helyre toostore ebben a projektben.

2. Használjon hello `mvn` parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.

   ```bash
   mvn archetype:generate -DgroupId=org.apache.hadoop.examples -DartifactId=wordcountjava -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
   ```

    > [!NOTE]
    > Ha a PowerShell használata esetén hello közé kell `-D` az idézőjelek közé foglalt paraméterek.
    >
    > `mvn archetype:generate "-DgroupId=org.apache.hadoop.examples" "-DartifactId=wordcountjava" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Ez a parancs létrehoz egy könyvtárat hello által megadott hello nevű `artifactID` paraméter (**wordcountjava** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:

   * `pom.xml`-hello [projekt Object Model (POM)](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html) , amely ismerteti, és konfigurációs részletek használt toobuild hello projekt.

   * `src`-hello alkalmazást tartalmazó hello könyvtár.

3. Törölje a hello `src/test/java/org/apache/hadoop/examples/apptest.java` fájlt. Ebben a példában nem szolgál.

## <a name="add-dependencies"></a>Adja hozzá a függőségek

1. Hello szerkesztése `pom.xml` fájlt, és adja hozzá a következő szöveget hello hello `<dependencies>` szakasz:
   
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

    Ez határozza meg a szükséges kódtárak (belül felsorolt &lt;artifactid szakaszát\>) megadott verzióval (belül felsorolt &lt;verzió\>). Fordítási időben a függőségek hello alapértelmezett Maven tárházból lesznek letöltve. Használhatja a hello [Maven tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hadoop%7Chadoop-mapreduce-examples%7C2.5.1%7Cjar) további tooview.
   
    Hello `<scope>provided</scope>` közli Maven, hogy az ezen függőségekhez nem csomagolható hello alkalmazással hello HDInsight-fürt futásidőben meghatározott.

    > [!IMPORTANT]
    > hello használt verziójának meg kell felelnie hello verziója található meg a fürt Hadoop. -Verziókban további információkért lásd: hello [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.

2. Adja hozzá a következő toohello hello `pom.xml` fájlt. Ez a szöveg hello belül kell `<project>...</project>` címkék hello fájlban, például közötti `</dependencies>` és `</project>`.

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

    hello első beépülő modul konfigurálása hello [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/), mely használt toobuild (más néven a fatjar), egy uberjar hello alkalmazáshoz szükséges függőségek tartalmazó van. Megakadályozza a párhuzamos licencek hello jar csomagon belül, amely az egyes rendszerek problémákat okozhat.

    hello második beépülő modul konfigurálása hello cél Java-verziót.

    > [!NOTE]
    > HDInsight 3.4-es és korábbi használat Java 7. HDInsight 3.5-ös és nagyobb Java 8 használja.

3. Mentse a hello `pom.xml` fájlt.

## <a name="create-hello-mapreduce-application"></a>Hello MapReduce-alkalmazás létrehozása

1. Nyissa meg toohello `wordcountjava/src/main/java/org/apache/hadoop/examples` könyvtár, és nevezze át hello `App.java` fájl túl`WordCount.java`.

2. Nyissa meg hello `WordCount.java` fájlt egy szövegszerkesztőben, és cserélje ki a következő szöveg hello hello tartalmát:
   
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
   
    Értesítés hello csomag neve `org.apache.hadoop.examples` hello osztály neve pedig `WordCount`. Amikor hello MapReduce feladatot használhatja ezeket a neveket.

3. Hello fájl mentéséhez.

## <a name="build-hello-application"></a>Hello alkalmazás létrehozása

1. Módosítsa a toohello `wordcountjava` könyvtárat, ha nem már létezik.

2. A következő parancs toobuild a JAR-fájlt tartalmazó hello alkalmazás hello használata:

   ```
   mvn clean package
   ```

    Ez a parancs törli az előző build összetevőihez, töltse le a bármely függőségek, amelyek nem már telepítve van, és ezután buildek és csomag hello alkalmazás.

3. Miután hello parancs futtatásának befejeztével hello `wordcountjava/target` directory nevű fájlt tartalmaz `wordcountjava-1.0-SNAPSHOT.jar`.
   
   > [!NOTE]
   > Hello `wordcountjava-1.0-SNAPSHOT.jar` fájl egy uberjar, nem csak a hello WordCount feladatot tartalmazó, de tartalomfüggőségeket, ezt a feladatot hello megköveteli azt is, futási időben.

## <a id="upload"></a>Hello jar feltöltése

A következő parancs tooupload hello jar fájl toohello HDInsight headnode hello használata:

   ```bash
   scp target/wordcountjava-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:
   ```

    Replace __USERNAME__ with your SSH user name for hello cluster. Replace __CLUSTERNAME__ with hello HDInsight cluster name.

Ez a parancs hello helyi rendszer toohello átjárócsomópont hello fájlokat másolja át. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="run"></a>A Hadoop hello MapReduce feladat futtatása

1. Csatlakozzon az SSH használatával tooHDInsight. További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

2. Hello SSH-munkamenetet használja a következő parancs toorun hello MapReduce alkalmazás hello:
   
   ```bash
   yarn jar wordcountjava-1.0-SNAPSHOT.jar org.apache.hadoop.examples.WordCount /example/data/gutenberg/davinci.txt /example/data/wordcountout
   ```
   
    Hello WordCount MapReduce alkalmazás indítja el. hello bemeneti fájl `/example/data/gutenberg/davinci.txt`, és a kimeneti könyvtár hello `/example/data/wordcountout`. Hello bemeneti fájl és a kimeneti tárolt toohello alapértelmezett hello fürt tárolóhelyét.

3. Amikor hello feladat befejeződik, használja a hello parancs tooview hello eredménye a következő:
   
   ```bash
   hdfs dfs -cat /example/data/wordcountout/*
   ```

    Szavak és számát, a szöveg a következő értékek hasonló toohello kell megjelennie:
   
        zeal    1
        zelus   1
        zenith  2

## <a id="nextsteps"></a>Következő lépések

Ebből a dokumentumból megtanulta, hogyan toodevelop Java MapReduce feladatot. Tekintse meg a következő dokumentumok a hdinsight eszközzel más módon toowork hello.

* [A Hive használata a HDInsightban][hdinsight-use-hive]
* [A Pig használata a HDInsightban][hdinsight-use-pig]
* [Use MapReduce with HDInsight](hdinsight-use-mapreduce.md)

További információkért lásd még: hello [Java fejlesztői központ](https://azure.microsoft.com/develop/java/).

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

