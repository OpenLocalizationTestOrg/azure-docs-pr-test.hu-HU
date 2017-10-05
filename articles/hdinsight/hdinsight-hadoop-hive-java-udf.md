---
title: "Java felhasználói függvény (UDF) a Hive HDInsight - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan hozzon létre egy Java-alapú felhasználói függvény (UDF), amely kompatibilis a struktúra. Ebben a példában UDF alakít egy szöveges karakterláncot kisbetűssé táblájában."
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
ms.openlocfilehash: 481d234eaf88bdb210821084ee4154159470eda0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-a-java-udf-with-hive-in-hdinsight"></a><span data-ttu-id="fef50-104">Egy Java használni a Hive HDInsight UDF</span><span class="sxs-lookup"><span data-stu-id="fef50-104">Use a Java UDF with Hive in HDInsight</span></span>

<span data-ttu-id="fef50-105">Megtudhatja, hogyan hozzon létre egy Java-alapú felhasználói függvény (UDF), amely kompatibilis a struktúra.</span><span class="sxs-lookup"><span data-stu-id="fef50-105">Learn how to create a Java-based user-defined function (UDF) that works with Hive.</span></span> <span data-ttu-id="fef50-106">Ebben a példában a Java UDF alakítja át a táblázatokat szöveges karakterláncok minden-kisbetűs karaktereket.</span><span class="sxs-lookup"><span data-stu-id="fef50-106">The Java UDF in this example converts a table of text strings to all-lowercase characters.</span></span>

## <a name="requirements"></a><span data-ttu-id="fef50-107">Követelmények</span><span class="sxs-lookup"><span data-stu-id="fef50-107">Requirements</span></span>

* <span data-ttu-id="fef50-108">HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="fef50-108">An HDInsight cluster</span></span> 

    > [!IMPORTANT]
    > <span data-ttu-id="fef50-109">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="fef50-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="fef50-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="fef50-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

    <span data-ttu-id="fef50-111">A legtöbb ebben a dokumentumban a lépések mindkét Windows és Linux-alapú fürtökön.</span><span class="sxs-lookup"><span data-stu-id="fef50-111">Most steps in this document work on both Windows- and Linux-based clusters.</span></span> <span data-ttu-id="fef50-112">A lefordított UDF feltöltése a fürthöz, majd futtassa a lépéseire azonban jellemzőek Linux-alapú fürtökön.</span><span class="sxs-lookup"><span data-stu-id="fef50-112">However, the steps used to upload the compiled UDF to the cluster and run it are specific to Linux-based clusters.</span></span> <span data-ttu-id="fef50-113">Információk a Windows-alapú fürtökön használható hivatkozásokkal.</span><span class="sxs-lookup"><span data-stu-id="fef50-113">Links are provided to information that can be used with Windows-based clusters.</span></span>

* <span data-ttu-id="fef50-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 vagy újabb (vagy egy azzal egyenértékű, például OpenJDK)</span><span class="sxs-lookup"><span data-stu-id="fef50-114">[Java JDK](http://www.oracle.com/technetwork/java/javase/downloads/) 8 or later (or an equivalent, such as OpenJDK)</span></span>

* [<span data-ttu-id="fef50-115">Apache Maven</span><span class="sxs-lookup"><span data-stu-id="fef50-115">Apache Maven</span></span>](http://maven.apache.org/)

* <span data-ttu-id="fef50-116">Szöveg- vagy Java IDE</span><span class="sxs-lookup"><span data-stu-id="fef50-116">A text editor or Java IDE</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="fef50-117">Ha egy Windows-ügyfélen a Python-fájlokat hoz létre, egy sor befejezési LF használó szerkesztővé kell használnia.</span><span class="sxs-lookup"><span data-stu-id="fef50-117">If you create the Python files on a Windows client, you must use an editor that uses LF as a line ending.</span></span> <span data-ttu-id="fef50-118">Ha nem biztos abban, hogy a szerkesztő LF vagy CRLF használja-e, tekintse meg a [hibaelhárítás](#troubleshooting) szakasz lépései a CR karakter eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="fef50-118">If you are not sure whether your editor uses LF or CRLF, see the [Troubleshooting](#troubleshooting) section for steps on removing the CR character.</span></span>

## <a name="create-an-example-java-udf"></a><span data-ttu-id="fef50-119">Hozzon létre egy Java UDF példa</span><span class="sxs-lookup"><span data-stu-id="fef50-119">Create an example Java UDF</span></span> 

1. <span data-ttu-id="fef50-120">A parancssorból használja a következő új Maven-projekt létrehozása:</span><span class="sxs-lookup"><span data-stu-id="fef50-120">From a command line, use the following to create a new Maven project:</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=ExampleUDF -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

   > [!NOTE]
   > <span data-ttu-id="fef50-121">Ha PowerShell használ, a paraméterek idézőjelbe kell helyezni.</span><span class="sxs-lookup"><span data-stu-id="fef50-121">If you are using PowerShell, you must put quotes around the parameters.</span></span> <span data-ttu-id="fef50-122">Például: `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span><span class="sxs-lookup"><span data-stu-id="fef50-122">For example, `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=ExampleUDF" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`.</span></span>

    <span data-ttu-id="fef50-123">Ezzel a paranccsal létrejön egy nevű könyvtár **exampleudf**, amely tartalmazza a Maven project.</span><span class="sxs-lookup"><span data-stu-id="fef50-123">This command creates a directory named **exampleudf**, which contains the Maven project.</span></span>

2. <span data-ttu-id="fef50-124">A projekt létrehozása után törölje a **exampleudf/src/test** a projekt részeként létrehozott címtárakat.</span><span class="sxs-lookup"><span data-stu-id="fef50-124">Once the project has been created, delete the **exampleudf/src/test** directory that was created as part of the project.</span></span>

3. <span data-ttu-id="fef50-125">Nyissa meg a **exampleudf/pom.xml**, és cserélje le a meglévő `<dependencies>` bejegyzés van a következő XML:</span><span class="sxs-lookup"><span data-stu-id="fef50-125">Open the **exampleudf/pom.xml**, and replace the existing `<dependencies>` entry with the following XML:</span></span>

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

    <span data-ttu-id="fef50-126">Ezek a bejegyzések adja meg a Hadoop és a Hive HDInsight 3.5 részét képező verzióját.</span><span class="sxs-lookup"><span data-stu-id="fef50-126">These entries specify the version of Hadoop and Hive included with HDInsight 3.5.</span></span> <span data-ttu-id="fef50-127">A Hadoop és a Hive HDInsight megadott verzióin információt a [HDInsight-összetevők verziószámozása](hdinsight-component-versioning.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="fef50-127">You can find information on the versions of Hadoop and Hive provided with HDInsight from the [HDInsight component versioning](hdinsight-component-versioning.md) document.</span></span>

    <span data-ttu-id="fef50-128">Adja hozzá a `<build>` előtt szakasz a `</project>` sort a fájl végén.</span><span class="sxs-lookup"><span data-stu-id="fef50-128">Add a `<build>` section before the `</project>` line at the end of the file.</span></span> <span data-ttu-id="fef50-129">Ez a szakasz a következő XML kell tartalmaznia:</span><span class="sxs-lookup"><span data-stu-id="fef50-129">This section should contain the following XML:</span></span>

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

    <span data-ttu-id="fef50-130">Ezek a bejegyzések meghatározhatja, hogyan kell a projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fef50-130">These entries define how to build the project.</span></span> <span data-ttu-id="fef50-131">Pontosabban a verzióját a projekt használó Java és hogyan hozhat létre egy uberjar a fürt üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="fef50-131">Specifically, the version of Java that the project uses and how to build an uberjar for deployment to the cluster.</span></span>

    <span data-ttu-id="fef50-132">Mentse a fájlt, miután a módosítások lettek bevezetve.</span><span class="sxs-lookup"><span data-stu-id="fef50-132">Save the file once the changes have been made.</span></span>

4. <span data-ttu-id="fef50-133">Nevezze át **exampleudf/src/main/java/com/microsoft/examples/App.java** való **ExampleUDF.java**, majd nyissa meg a fájlt a szerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="fef50-133">Rename **exampleudf/src/main/java/com/microsoft/examples/App.java** to **ExampleUDF.java**, and then open the file in your editor.</span></span>

5. <span data-ttu-id="fef50-134">Cserélje le a tartalmát a **ExampleUDF.java** a következő fájlt, majd mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="fef50-134">Replace the contents of the **ExampleUDF.java** file with the following, then save the file.</span></span>

    ```java
    package com.microsoft.examples;

    import org.apache.hadoop.hive.ql.exec.Description;
    import org.apache.hadoop.hive.ql.exec.UDF;
    import org.apache.hadoop.io.*;

    // Description of the UDF
    @Description(
        name="ExampleUDF",
        value="returns a lower case version of the input string.",
        extended="select ExampleUDF(deviceplatform) from hivesampletable limit 10;"
    )
    public class ExampleUDF extends UDF {
        // Accept a string input
        public String evaluate(String input) {
            // If the value is null, return a null
            if(input == null)
                return null;
            // Lowercase the input string and return it
            return input.toLowerCase();
        }
    }
    ```

    <span data-ttu-id="fef50-135">Ez a kód egy UDF fogad el egy olyan karakterláncértéket, és a karakterlánc kis verziójának visszaadó valósítja meg.</span><span class="sxs-lookup"><span data-stu-id="fef50-135">This code implements a UDF that accepts a string value, and returns a lowercase version of the string.</span></span>

## <a name="build-and-install-the-udf"></a><span data-ttu-id="fef50-136">Hozza létre és telepítse az UDF-ben</span><span class="sxs-lookup"><span data-stu-id="fef50-136">Build and install the UDF</span></span>

1. <span data-ttu-id="fef50-137">A következő paranccsal fordításához és az UDF csomag:</span><span class="sxs-lookup"><span data-stu-id="fef50-137">Use the following command to compile and package the UDF:</span></span>

    ```bash
    mvn compile package
    ```

    <span data-ttu-id="fef50-138">Ez a parancs létrehozza, és az UDF-csomagok a `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` fájlt.</span><span class="sxs-lookup"><span data-stu-id="fef50-138">This command builds and packages the UDF into the `exampleudf/target/ExampleUDF-1.0-SNAPSHOT.jar` file.</span></span>

2. <span data-ttu-id="fef50-139">Használja a `scp` parancs a fájl átmásolása a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="fef50-139">Use the `scp` command to copy the file to the HDInsight cluster.</span></span>

    ```bash
    scp ./target/ExampleUDF-1.0-SNAPSHOT.jar myuser@mycluster-ssh.azurehdinsight
    ```

    <span data-ttu-id="fef50-140">Cserélje le `myuser` a fürthöz SSH felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="fef50-140">Replace `myuser` with the SSH user account for your cluster.</span></span> <span data-ttu-id="fef50-141">Cserélje le `mycluster` a fürt nevéhez.</span><span class="sxs-lookup"><span data-stu-id="fef50-141">Replace `mycluster` with the cluster name.</span></span> <span data-ttu-id="fef50-142">Ha a jelszót biztonságos SSH-fiókjának biztonságát, a jelszó megadására kéri.</span><span class="sxs-lookup"><span data-stu-id="fef50-142">If you used a password to secure the SSH account, you are prompted to enter the password.</span></span> <span data-ttu-id="fef50-143">Ha a tanúsítványt használja, előfordulhat, hogy szüksége a `-i` paraméterrel adhatja meg a titkos kulcs fájlja.</span><span class="sxs-lookup"><span data-stu-id="fef50-143">If you used a certificate, you may need to use the `-i` parameter to specify the private key file.</span></span>

3. <span data-ttu-id="fef50-144">Csatlakozzon a fürthöz SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="fef50-144">Connect to the cluster using SSH.</span></span>

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="fef50-145">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="fef50-145">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

4. <span data-ttu-id="fef50-146">Az SSH-munkamenetet másolja a jar-fájlra HDInsight-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="fef50-146">From the SSH session, copy the jar file to HDInsight storage.</span></span>

    ```bash
    hdfs dfs -put ExampleUDF-1.0-SNAPSHOT.jar /example/jars
    ```

## <a name="use-the-udf-from-hive"></a><span data-ttu-id="fef50-147">Az UDF-ben a Hive használata</span><span class="sxs-lookup"><span data-stu-id="fef50-147">Use the UDF from Hive</span></span>

1. <span data-ttu-id="fef50-148">A következő segítségével indítsa el a Beeline ügyfél az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="fef50-148">Use the following to start the Beeline client from the SSH session.</span></span>

    ```bash
    beeline -u 'jdbc:hive2://localhost:10001/;transportMode=http' -n admin
    ```

    <span data-ttu-id="fef50-149">A parancs feltételezi, hogy használja-e a rendszer az alapértelmezett **admin** a bejelentkezési fiók a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="fef50-149">This command assumes that you used the default of **admin** for the login account for your cluster.</span></span>

2. <span data-ttu-id="fef50-150">Miután a kiszolgálófarmban lévő a `jdbc:hive2://localhost:10001/>` kéri, írja be a következő Hive UDF és közzétenni függvényében.</span><span class="sxs-lookup"><span data-stu-id="fef50-150">Once you arrive at the `jdbc:hive2://localhost:10001/>` prompt, enter the following to add the UDF to Hive and expose it as a function.</span></span>

    ```hiveql
    ADD JAR wasb:///example/jars/ExampleUDF-1.0-SNAPSHOT.jar;
    CREATE TEMPORARY FUNCTION tolower as 'com.microsoft.examples.ExampleUDF';
    ```

    > [!NOTE]
    > <span data-ttu-id="fef50-151">Ez a példa feltételezi, hogy a fürt tárolóhelyét alapértelmezett Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="fef50-151">This example assumes that Azure Storage is default storage for the cluster.</span></span> <span data-ttu-id="fef50-152">Ha a fürt helyette használja a Data Lake Store, módosítsa a `wasb:///` egy érték `adl:///`.</span><span class="sxs-lookup"><span data-stu-id="fef50-152">If your cluster uses Data Lake Store instead, change the `wasb:///` value to `adl:///`.</span></span>

3. <span data-ttu-id="fef50-153">Az UDF segítségével olvassa be az táblából kisbetű karakterláncok értékeket átalakítani.</span><span class="sxs-lookup"><span data-stu-id="fef50-153">Use the UDF to convert values retrieved from a table to lower case strings.</span></span>

    ```hiveql
    SELECT tolower(deviceplatform) FROM hivesampletable LIMIT 10;
    ```

    <span data-ttu-id="fef50-154">Ez a lekérdezés kiválasztja az eszköz platformjától (Android, Windows, iOS, stb.) a táblából, alakítsa át a alacsonyabb eset, és majd megjeleníti a karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="fef50-154">This query selects the device platform (Android, Windows, iOS, etc.) from the table, convert the string to lower case, and then display them.</span></span> <span data-ttu-id="fef50-155">A kimenet az alábbihoz hasonló jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="fef50-155">The output appears similar to the following text:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="fef50-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fef50-156">Next steps</span></span>

<span data-ttu-id="fef50-157">Más módokon történő együttműködésre a Hive, lásd: [használata a HDInsight Hive](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="fef50-157">For other ways to work with Hive, see [Use Hive with HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="fef50-158">Hive User-Defined funkciók további információkért lásd: [Hive operátor és a felhasználó által definiált függvényeket](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) szakaszt a Hive wiki az Apache.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="fef50-158">For more information on Hive User-Defined Functions, see [Hive Operators and User-Defined Functions](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+UDF) section of the Hive wiki at apache.org.</span></span>
