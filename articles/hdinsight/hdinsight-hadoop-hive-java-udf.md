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
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="58c31-104">Egy Java használni a Hive HDInsight UDF</span><span class="sxs-lookup"><span data-stu-id="58c31-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="58c31-105">Ismerje meg, hogyan toocreate Java-alapú felhasználói függvény (UDF), amely kompatibilis a struktúra.</span><span class="sxs-lookup"><span data-stu-id="58c31-105">Learn how toocreate a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="58c31-106">hello Java UDF ebben a példában egy tábla szöveges karakterláncok tooall-kisbetűs karaktereket alakítja át.</span><span class="sxs-lookup"><span data-stu-id="58c31-106">hello Java UDF in this example converts a table of text strings tooall-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="58c31-107">Követelmények</span><span class="sxs-lookup"><span data-stu-id="58c31-107">Requirements</span></span>

* <span data-ttu-id="58c31-108">HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="58c31-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="58c31-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="58c31-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="58c31-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="58c31-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="58c31-111">A legtöbb ebben a dokumentumban a lépések mindkét Windows és Linux-alapú fürtökön.</span><span class="sxs-lookup"><span data-stu-id="58c31-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="58c31-112">Hello használt lépéseket tooupload hello fordítása azonban UDF toohello fürt és az adott tooLinux-alapú fürtök futtatja.</span><span class="sxs-lookup"><span data-stu-id="58c31-112">However, hello steps used tooupload hello compiled UDF toohello cluster and run it are specific tooLinux-based clusters.</span></span> <span data-ttu-id="58c31-113">A Windows-alapú fürtökön használható tooinformation hivatkozásokkal.</span><span class="sxs-lookup"><span data-stu-id="58c31-113">Links are provided tooinformation that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="58c31-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 vagy újabb (vagy egy azzal egyenértékű, például OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="58c31-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="58c31-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="58c31-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="58c31-116">Szöveg- vagy Java IDE</span><span class="sxs-lookup"><span data-stu-id="58c31-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="58c31-117">Hello egy Windows ügyfél Python-fájlok létrehozása, ha egy sor befejezési LF használó szerkesztővé kell használnia.</span><span class="sxs-lookup"><span data-stu-id="58c31-117">If you create hello Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="58c31-118">Ha nem biztos abban, hogy a szerkesztő LF vagy CRLF használja-e, tekintse meg a hello [hibaelhárítás](#troubleshooting) a lépésekkel eltávolítása a CR karakter hello szakasz.</span><span class="sxs-lookup"><span data-stu-id="58c31-118">If you are not sure whether your editor uses LF or CRLF, see hello [Troubleshooting](#troubleshooting) section for steps on removing hello CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="58c31-119">Hozzon létre egy Java UDF példa</span><span class="sxs-lookup"><span data-stu-id="58c31-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="58c31-120">A parancssorból a következő új Maven-projektté toocreate hello használata:</span><span class="sxs-lookup"><span data-stu-id="58c31-120">From a command line, use hello following toocreate a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="58c31-121">Ha a PowerShell használata esetén hello paraméterek idézőjelbe kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="58c31-121">If you are using PowerShell, you must put quotes around hello parameters.</span></span> <span data-ttu-id="58c31-122">Például: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="58c31-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="58c31-123">Ezzel a paranccsal létrejön egy nevű könyvtár **exampleudf**, amely tartalmazza a hello Maven project.</span><span class="sxs-lookup"><span data-stu-id="58c31-123">This command creates a directory named **exampleudf**, which contains hello Maven project.</span></span>

2. <span data-ttu-id="58c31-124">Hello projekt létrehozása, törlése hello **exampleudf/src/test** hello projekt részeként létrehozott címtárakat.</span><span class="sxs-lookup"><span data-stu-id="58c31-124">Once hello project has been created, delete hello **exampleudf/src/test** directory that was created as part of hello project.</span></span>

3. <span data-ttu-id="58c31-125">Megnyitás hello **exampleudf/pom.xml**, és cserélje le a meglévő hello `<dependencies>` hello XML a következő bejegyzést:</span><span class="sxs-lookup"><span data-stu-id="58c31-125">Open hello **exampleudf/pom.xml**, and replace hello existing `<dependencies>` entry with hello following XML:</span></span>

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

    <span data-ttu-id="58c31-126">Ezek a bejegyzések adja meg a Hadoop és a Hive HDInsight 3.5 mellékelt hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="58c31-126">These entries specify hello version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="58c31-127">A Hadoop és a Hive hdinsight kapott hello hello verzióin információt [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="58c31-127">You can find information on hello versions of Hadoop and Hive provided with HDInsight from hello [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="58c31-128">Adja hozzá a `<build>` előtt hello szakasz `</project>` sor hello fájl hello végén.</span><span class="sxs-lookup"><span data-stu-id="58c31-128">Add a `<build>` section before hello `</project>` line at hello end of hello file.</span></span> <span data-ttu-id="58c31-129">Ez a szakasz a következő XML hello kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="58c31-129">This section should contain hello following XML:</span></span>

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

    <span data-ttu-id="58c31-130">Ezek a bejegyzések határozza meg, hogyan toobuild hello projekt.</span><span class="sxs-lookup"><span data-stu-id="58c31-130">These entries define how toobuild hello project.</span></span> <span data-ttu-id="58c31-131">Java, amely a projekt által használt hello pontosabban hello verzióját, és hogyan toobuild egy uberjar telepítési toohello fürthöz.</span><span class="sxs-lookup"><span data-stu-id="58c31-131">Specifically, hello version of Java that hello project uses and how toobuild an uberjar for deployment toohello cluster.</span></span>

    <span data-ttu-id="58c31-132">Miután hello módosításokat végzett, mentse a hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="58c31-132">Save hello file once hello changes have been made.</span></span>

4. <span data-ttu-id="58c31-133">Nevezze át **exampleudf/src/main/java/com/microsoft/examples/App.java** túl**ExampleUDF.java**, majd nyissa meg a szerkesztőben hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="58c31-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** too**ExampleUDF.java**, and then open hello file in your editor.</span></span>

5. <span data-ttu-id="58c31-134">Cserélje le a hello hello tartalmát **ExampleUDF.java** hello következő fájlt, majd mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="58c31-134">Replace hello contents of hello **ExampleUDF.java** file with hello following, then save hello file.</span></span>

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

    <span data-ttu-id="58c31-135">Ez a kód egy karakterlánc-érték fogad, és hello karakterlánc kisbetűs verzióját visszaadó UDF valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="58c31-135">This code implements a UDF that accepts a string value, and returns a lowercase version of hello string.</span></span>

## <a name="build-and-install-hello-udf"></a><span data-ttu-id="58c31-136">Hozza létre és hello UDF telepítése</span><span class="sxs-lookup"><span data-stu-id="58c31-136">Build and install hello UDF</span></span>

1. <span data-ttu-id="58c31-137">A következő parancs toocompile hello használja, és hello UDF csomag:</span><span class="sxs-lookup"><span data-stu-id="58c31-137">Use hello following command toocompile and package hello UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="58c31-138">Ez a parancs létrehozza és csomagok hello UDF-ben történő hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` fájlt.</span><span class="sxs-lookup"><span data-stu-id="58c31-138">This command builds and packages hello UDF into hello `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="58c31-139">Használjon hello `scp` parancs toocopy hello fájl toohello HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="58c31-139">Use hello `scp` command toocopy hello file toohello HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="58c31-140">Cserélje le `myuser` a hello SSH felhasználói fiók a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="58c31-140">Replace `myuser` with hello SSH user account for your cluster.</span></span> <span data-ttu-id="58c31-141">Cserélje le `mycluster` hello fürt névvel.</span><span class="sxs-lookup"><span data-stu-id="58c31-141">Replace `mycluster` with hello cluster name.</span></span> <span data-ttu-id="58c31-142">Ha a jelszó toosecure hello SSH-fiókjának, felszólító tooenter hello jelszó áll.</span><span class="sxs-lookup"><span data-stu-id="58c31-142">If you used a password toosecure hello SSH account, you are prompted tooenter hello password.</span></span> <span data-ttu-id="58c31-143">Ha a tanúsítványt használja, szükség lehet a toouse hello `-i` paraméter toospecify hello titkos kulcs fájlja.</span><span class="sxs-lookup"><span data-stu-id="58c31-143">If you used a certificate, you may need toouse hello `-i` parameter toospecify hello private key file.</span></span>

3. <span data-ttu-id="58c31-144">Csatlakozzon az SSH használatával toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="58c31-144">Connect toohello cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="58c31-145">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="58c31-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="58c31-146">Hello SSH-munkamenetet másolja a hello jar fájlok tooHDInsight tárolására.</span><span class="sxs-lookup"><span data-stu-id="58c31-146">From hello SSH session, copy hello jar file tooHDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-hello-udf-from-hive"></a><span data-ttu-id="58c31-147">A Hive hello UDF használata</span><span class="sxs-lookup"><span data-stu-id="58c31-147">Use hello UDF from Hive</span></span>

1. <span data-ttu-id="58c31-148">A következő toostart hello Beeline ügyfél hello SSH-munkamenetből hello használja.</span><span class="sxs-lookup"><span data-stu-id="58c31-148">Use hello following toostart hello Beeline client from hello SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="58c31-149">A parancs feltételezi, hogy használja-e hello alapértelmezett **admin** hello bejelentkezési fiók a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="58c31-149">This command assumes that you used hello default of **admin** for hello login account for your cluster.</span></span>

2. <span data-ttu-id="58c31-150">Miután megérkezik a hello `jdbc:hive2://localhost:10001/>` parancssorba írja be a következő tooadd hello UDF tooHive hello és közzétenni függvényében.</span><span class="sxs-lookup"><span data-stu-id="58c31-150">Once you arrive at hello `jdbc:hive2://localhost:10001/>` prompt, enter hello following tooadd hello UDF tooHive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="58c31-151">Ez a példa feltételezi, hogy Azure Storage alapértelmezett hello fürt tárolóhelyét.</span><span class="sxs-lookup"><span data-stu-id="58c31-151">This example assumes that Azure Storage is default storage for hello cluster.</span></span> <span data-ttu-id="58c31-152">Ha a fürt helyette használja a Data Lake Store, módosítsa a hello `wasb:///` érték túl`adl:///`.</span><span class="sxs-lookup"><span data-stu-id="58c31-152">If your cluster uses Data Lake Store instead, change hello `wasb:///` value too`adl:///`.</span></span>

3. <span data-ttu-id="58c31-153">Egy tábla toolower eset karakterláncok lekért hello UDF tooconvert értékeket használja.</span><span class="sxs-lookup"><span data-stu-id="58c31-153">Use hello UDF tooconvert values retrieved from a table toolower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="58c31-154">Ez a lekérdezés választják ki hello eszközplatform (Android, Windows, iOS, stb.) hello táblából, alakítsa át a hello karakterlánc toolower eset, majd megjeleníteni azokat.</span><span class="sxs-lookup"><span data-stu-id="58c31-154">This query selects hello device platform (Android, Windows, iOS, etc.) from hello table, convert hello string toolower case, and then display them.</span></span> <span data-ttu-id="58c31-155">hello eredmény jelenik meg a következő szöveg hasonló toohello:</span><span class="sxs-lookup"><span data-stu-id="58c31-155">hello output appears similar toohello following text:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="58c31-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58c31-156">Next steps</span></span>

<span data-ttu-id="58c31-157">Más módokon toowork a Hive, lásd: [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="58c31-157">For other ways toowork with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="58c31-158">Hive User-Defined funkciók további információkért lásd: [Hive operátor és a felhasználó által definiált függvényeket](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) szakasz hello Hive wiki az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="58c31-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of hello Hive wiki at apache.org.</span></span>
