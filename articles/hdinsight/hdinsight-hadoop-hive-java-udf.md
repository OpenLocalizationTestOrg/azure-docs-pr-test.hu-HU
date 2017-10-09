---
title: "aaaJava felhasználói függvény (UDF) a Hive HDInsight - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Java-alapú felhasználói függvény (UDF), amely kompatibilis a struktúra. Ez a példa UDF szöveges karakterláncok toolowercase táblázatát alakítja át."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 8d4f8efe-2f01-4a61-8619-651e873c7982
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 392b4cfb73299d2f6c1e8e825a4201b48d501388
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a>Egy Java használni a Hive HDInsight UDF

Ismerje meg, hogyan toocreate Java-alapú felhasználói függvény (UDF), amely kompatibilis a struktúra. hello Java UDF ebben a példában egy tábla szöveges karakterláncok tooall-kisbetűs karaktereket alakítja át.

## <a name="requirements"></a>Követelmények

* HDInsight-fürtök 

    > [!IMPORTANT]
    > Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

    A legtöbb ebben a dokumentumban a lépések mindkét Windows és Linux-alapú fürtökön. Hello használt lépéseket tooupload hello fordítása azonban UDF toohello fürt és az adott tooLinux-alapú fürtök futtatja. A Windows-alapú fürtökön használható tooinformation hivatkozásokkal.

* [Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 vagy újabb (vagy egy azzal egyenértékű, például OpenJDK)

* [Apache Maven](http://maven.apache.org/)

* Szöveg- vagy Java IDE

    > [!IMPORTANT]
    > Hello egy Windows ügyfél Python-fájlok létrehozása, ha egy sor befejezési LF használó szerkesztővé kell használnia. Ha nem biztos abban, hogy a szerkesztő LF vagy CRLF használja-e, tekintse meg a hello [hibaelhárítás](#troubleshooting) a lépésekkel eltávolítása a CR karakter hello szakasz.

## <a name="create-an-example-java-udf"></a>Hozzon létre egy Java UDF példa 

1. A parancssorból a következő új Maven-projektté toocreate hello használata:

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > Ha a PowerShell használata esetén hello paraméterek idézőjelbe kell helyezni. Például: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.

    Ezzel a paranccsal létrejön egy nevű könyvtár **exampleudf**, amely tartalmazza a hello Maven project.

2. Hello projekt létrehozása, törlése hello **exampleudf/src/test** hello projekt részeként létrehozott címtárakat.

3. Megnyitás hello **exampleudf/pom.xml**, és cserélje le a meglévő hello `<dependencies>` hello XML a következő bejegyzést:

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.7.3</version>
            <scope>provided</scope>
        </dependency>
        <dependency>
            <groupId>org.apache.hive</groupId>
            <artifactId>hive-exec</artifactId>
            <version>1.2.1</version>
            <scope>provided</scope>
        </dependency>
    </dependencies>
    ```

    Ezek a bejegyzések adja meg a Hadoop és a Hive HDInsight 3.5 mellékelt hello verzióját. A Hadoop és a Hive hdinsight kapott hello hello verzióin információt [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.

    Adja hozzá a `<build>` előtt hello szakasz `</project>` sor hello fájl hello végén. Ez a szakasz a következő XML hello kell tartalmaznia:

    ```xml
    <build>
        <plugins>
            <!-- build for Java 1.8. This is required by HDInsight 3.5  -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.3</version>
                <configuration>
                    <source>1.8</source>
                    <target>1.8</target>
                </configuration>
            </plugin>
            <!-- build an uber jar -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>2.3</version>
                <configuration>
                    <!-- Keep us from getting a can't overwrite file error -->
                    <transformers>
                        <transformer
                                implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                        </transformer>
                        <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer">
                        </transformer>
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
        </plugins>
    </build>
    ```

    Ezek a bejegyzések határozza meg, hogyan toobuild hello projekt. Java, amely a projekt által használt hello pontosabban hello verzióját, és hogyan toobuild egy uberjar telepítési toohello fürthöz.

    Miután hello módosításokat végzett, mentse a hello fájlt.

4. Nevezze át **exampleudf/src/main/java/com/microsoft/examples/App.java** túl**ExampleUDF.java**, majd nyissa meg a szerkesztőben hello fájlt.

5. Cserélje le a hello hello tartalmát **ExampleUDF.java** hello következő fájlt, majd mentse hello fájlt.

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of hello UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of hello input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If hello value is null, return a null
            if(input == null)
                return null;
            // Lowercase hello input string and return it
            return input.toLowerCase();
        }
    }
    ```

    Ez a kód egy karakterlánc-érték fogad, és hello karakterlánc kisbetűs verzióját visszaadó UDF valósítja meg.

## <a name="build-and-install-hello-udf"></a>Hozza létre és hello UDF telepítése

1. A következő parancs toocompile hello használja, és hello UDF csomag:

    ```bash
    mvn compile package
    ```

    Ez a parancs létrehozza és csomagok hello UDF-ben történő hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` fájlt.

2. Használjon hello `scp` parancs toocopy hello fájl toohello HDInsight-fürthöz.

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    Cserélje le `myuser` a hello SSH felhasználói fiók a fürt számára. Cserélje le `mycluster` hello fürt névvel. Ha a jelszó toosecure hello SSH-fiókjának, felszólító tooenter hello jelszó áll. Ha a tanúsítványt használja, szükség lehet a toouse hello `-i` paraméter toospecify hello titkos kulcs fájlja.

3. Csatlakozzon az SSH használatával toohello fürt.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

4. Hello SSH-munkamenetet másolja a hello jar fájlok tooHDInsight tárolására.

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a>A Hive hello UDF használata

1. A következő toostart hello Beeline ügyfél hello SSH-munkamenetből hello használja.

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    A parancs feltételezi, hogy használja-e hello alapértelmezett **admin** hello bejelentkezési fiók a fürt számára.

2. Miután megérkezik a hello `jdbc:hive2://localhost:10001/>` parancssorba írja be a következő tooadd hello UDF tooHive hello és közzétenni függvényében.

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > Ez a példa feltételezi, hogy Azure Storage alapértelmezett hello fürt tárolóhelyét. Ha a fürt helyette használja a Data Lake Store, módosítsa a hello `wasb:///` érték túl`adl:///`.

3. Egy tábla toolower eset karakterláncok lekért hello UDF tooconvert értékeket használja.

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    Ez a lekérdezés választják ki hello eszközplatform (Android, Windows, iOS, stb.) hello táblából, alakítsa át a hello karakterlánc toolower eset, majd megjeleníteni azokat. hello eredmény jelenik meg a következő szöveg hasonló toohello:

        +----------+--+
        |   _c0    |
        +----------+--+
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        | android  |
        +----------+--+

## <a name="next-steps"></a>Következő lépések

Más módokon toowork a Hive, lásd: [használata a HDInsight Hive](hdinsight-use-hive.md).

Hive User-Defined funkciók további információkért lásd: [Hive operátor és a felhasználó által definiált függvényeket](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) szakasz hello Hive wiki az Apache.org webhelyen.
