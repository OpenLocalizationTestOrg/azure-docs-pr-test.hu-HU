---
title: "aaaJava HBase ügyfél - Azure HDInsight |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Apache Maven Java-alapú toobuild Apache HBase alkalmazást, majd telepítenie kell azt az Azure HDInsight tooHBase."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: 
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 41ef92b2900280dd59089c4fa40686c44133b337
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-java-applications-for-apache-hbase"></a><span data-ttu-id="da685-103">Az Apache HBase Java-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="da685-103">Build Java applications for Apache HBase</span></span>

<span data-ttu-id="da685-104">Megtudhatja, hogyan toocreate egy [Apache HBase](http://hbase.apache.org/) alkalmazás Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="da685-104">Learn how toocreate an [Apache HBase](http://hbase.apache.org/) application in Java.</span></span> <span data-ttu-id="da685-105">Majd használni az Azure HDInsight HBase hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="da685-105">Then use hello application with HBase on Azure HDInsight.</span></span>

<span data-ttu-id="da685-106">Ez a dokumentum használatát a lépések hello [Maven](http://maven.apache.org/) toocreate és -buildek hello projekt.</span><span class="sxs-lookup"><span data-stu-id="da685-106">hello steps in this document use [Maven](http://maven.apache.org/) toocreate and build hello project.</span></span> <span data-ttu-id="da685-107">Maven egy a projekt szoftverkezelés és toobuild szoftver, dokumentáció és jelentések Java-projektek esetében érthetőséget eszközzel.</span><span class="sxs-lookup"><span data-stu-id="da685-107">Maven is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span>

> [!NOTE]
> <span data-ttu-id="da685-108">hello jelen dokumentumban leírt lépések alapján legutóbb történő teszteléskor az HDInsight 3.6.</span><span class="sxs-lookup"><span data-stu-id="da685-108">hello steps in this document were most recently tested with HDInsight 3.6.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="da685-109">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek.</span><span class="sxs-lookup"><span data-stu-id="da685-109">hello steps in this document require an HDInsight cluster that uses Linux.</span></span> <span data-ttu-id="da685-110">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="da685-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="da685-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="da685-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="da685-112">Követelmények</span><span class="sxs-lookup"><span data-stu-id="da685-112">Requirements</span></span>

* <span data-ttu-id="da685-113">[Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="da685-113">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 or later.</span></span>

    > [!NOTE]
    > <span data-ttu-id="da685-114">HDInsight 3.5-ös és nagyobb Java 8 igényel.</span><span class="sxs-lookup"><span data-stu-id="da685-114">HDInsight 3.5 and greater requires Java 8.</span></span> <span data-ttu-id="da685-115">HDInsight a korábbi Java 7 igényelnek.</span><span class="sxs-lookup"><span data-stu-id="da685-115">Earlier versions of HDInsight require Java 7.</span></span>

* [<span data-ttu-id="da685-116">Maven 3</span><span class="sxs-lookup"><span data-stu-id="da685-116">Maven</span></span>](http://maven.apache.org/)

* [<span data-ttu-id="da685-117">A HBase Linux-alapú Azure HDInsight fürt</span><span class="sxs-lookup"><span data-stu-id="da685-117">A Linux-based Azure HDInsight cluster with HBase</span></span>](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > <span data-ttu-id="da685-118">jelen dokumentumban leírt lépések hello HDInsight fürt 3.4 és verziójú 3.5 lettek tesztelve.</span><span class="sxs-lookup"><span data-stu-id="da685-118">hello steps in this document have been tested with HDInsight cluster versions 3.4 and 3.5.</span></span> <span data-ttu-id="da685-119">hello alapértelmezett példákban megadott értékei HDInsight 3.5 fürt.</span><span class="sxs-lookup"><span data-stu-id="da685-119">hello default values provided in examples are for a HDInsight 3.5 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="da685-120">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="da685-120">Create hello project</span></span>

1. <span data-ttu-id="da685-121">Parancssorból hello a fejlesztési környezetben, módosítsa a könyvtárakat toohello helyének toocreate hello projekt, például `cd code\hbase`.</span><span class="sxs-lookup"><span data-stu-id="da685-121">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hbase`.</span></span>

2. <span data-ttu-id="da685-122">Használjon hello **mvn** parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.</span><span class="sxs-lookup"><span data-stu-id="da685-122">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > <span data-ttu-id="da685-123">Ha a PowerShell használata esetén hello közé kell `-D` az idézőjelek közé foglalt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="da685-123">If you are using PowerShell, you must enclose hello `-D` parameters in double quotes.</span></span>
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    <span data-ttu-id="da685-124">Ez a parancs létrehoz egy könyvtárat az azonos nevet, amint hello hello **artifactid szakaszát** paraméter (**hbaseapp** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="da685-124">This command creates a directory with hello same name as hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="da685-125">**pom.xml**: hello projekt Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) konfigurációban használt toobuild hello projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="da685-125">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="da685-126">**src**: hello tartalmazó hello könyvtár **main/java/com/microsoft/példák** könyvtár, amikor szerzőként hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="da685-126">**src**: hello directory that contains hello **main/java/com/microsoft/examples** directory, where you author hello application.</span></span>

3. <span data-ttu-id="da685-127">Törölje a hello `src/test/java/com/microsoft/examples/apptest.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-127">Delete hello `src/test/java/com/microsoft/examples/apptest.java` file.</span></span> <span data-ttu-id="da685-128">Nem használható ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="da685-128">It is not be used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="da685-129">Frissítés hello projekt Hálózatiobjektum-modellje</span><span class="sxs-lookup"><span data-stu-id="da685-129">Update hello Project Object Model</span></span>

1. <span data-ttu-id="da685-130">Hello szerkesztése `pom.xml` fájlt, és adja hozzá a következő kódot hello hello `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="da685-130">Edit hello `pom.xml` file and add hello following code inside hello `<dependencies>` section:</span></span>

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    <span data-ttu-id="da685-131">Ez a szakasz azt jelzi, hogy hello projekt be kell **hbase-ügyfél** és **phoenix-core** összetevőket.</span><span class="sxs-lookup"><span data-stu-id="da685-131">This section indicates that hello project needs **hbase-client** and **phoenix-core** components.</span></span> <span data-ttu-id="da685-132">Fordítási időben a függőségek hello alapértelmezett Maven tárházból lesznek letöltve.</span><span class="sxs-lookup"><span data-stu-id="da685-132">At compile time, these dependencies are downloaded from hello default Maven repository.</span></span> <span data-ttu-id="da685-133">Használhatja a hello [Maven központi tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) további információk a függőség toolearn.</span><span class="sxs-lookup"><span data-stu-id="da685-133">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="da685-134">hello hbase-ügyfél verziószáma hello egyeznie kell a HBase a HDInsight-fürthöz biztosított hello verziójával.</span><span class="sxs-lookup"><span data-stu-id="da685-134">hello version number of hello hbase-client must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="da685-135">A következő tábla toofind hello megfelelő verziószámot hello használata.</span><span class="sxs-lookup"><span data-stu-id="da685-135">Use hello following table toofind hello correct version number.</span></span>

   | <span data-ttu-id="da685-136">HDInsight-fürt verziószáma</span><span class="sxs-lookup"><span data-stu-id="da685-136">HDInsight cluster version</span></span> | <span data-ttu-id="da685-137">A HBase verzió toouse</span><span class="sxs-lookup"><span data-stu-id="da685-137">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="da685-138">3.2</span><span class="sxs-lookup"><span data-stu-id="da685-138">3.2</span></span> |<span data-ttu-id="da685-139">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="da685-139">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="da685-140">3.3-as, 3.4, 3.5-ös és 3.6.</span><span class="sxs-lookup"><span data-stu-id="da685-140">3.3, 3.4, 3.5, and 3.6</span></span> |<span data-ttu-id="da685-141">1.1.2</span><span class="sxs-lookup"><span data-stu-id="da685-141">1.1.2</span></span> |

    <span data-ttu-id="da685-142">A HDInsight-verziókról és összetevők további információkért lásd: [hello különböző Hadoop-összetevők és a HDInsight együttes rendelkezésre Mik](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="da685-142">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>

3. <span data-ttu-id="da685-143">Adja hozzá a következő kód toohello hello **pom.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-143">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="da685-144">Ez a szöveg hello belül kell `<project>...</project>` hello címkék fájlba, például közötti `</dependencies>` és `</project>`.</span><span class="sxs-lookup"><span data-stu-id="da685-144">This text must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

   ```xml
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
        <resource>
            <directory>${basedir}/conf</directory>
            <filtering>false</filtering>
            <includes>
            <include>hbase-site.xml</include>
            </includes>
        </resource>
        </resources>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
            </plugin>
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
        </plugins>
    </build>
   ```

    <span data-ttu-id="da685-145">Ez a szakasz konfigurál egy erőforrást (`conf/hbase-site.xml`), amely a HBase konfigurációs információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="da685-145">This section configures a resource (`conf/hbase-site.xml`) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="da685-146">Megadhatja a konfigurációs értékeket keresztül kódot is.</span><span class="sxs-lookup"><span data-stu-id="da685-146">You can also set configuration values via code.</span></span> <span data-ttu-id="da685-147">A hello hello megjegyzéseket `CreateTable` példa.</span><span class="sxs-lookup"><span data-stu-id="da685-147">See hello comments in hello `CreateTable` example.</span></span>

    <span data-ttu-id="da685-148">Ez a szakasz is konfigurálja hello [Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="da685-148">This section also configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="da685-149">hello fordító beépülő modul használt toocompile hello topológia.</span><span class="sxs-lookup"><span data-stu-id="da685-149">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="da685-150">hello árnyalatát beépülő modul használt tooprevent licenc ismétlődést hello JAR package Maven által épített.</span><span class="sxs-lookup"><span data-stu-id="da685-150">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="da685-151">A beépülő modul futási időben hello HDInsight-fürt "ismétlődő licencfájlok" hiba használt tooprevent van.</span><span class="sxs-lookup"><span data-stu-id="da685-151">This plugin is used tooprevent a "duplicate license files" error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="da685-152">Maven-árnyalatát-beépülő modul használata hello `ApacheLicenseResourceTransformer` megvalósítási megakadályozza, hogy a hello hiba.</span><span class="sxs-lookup"><span data-stu-id="da685-152">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents hello error.</span></span>

    <span data-ttu-id="da685-153">maven-árnyalatát-beépülő modul hello is egy uber jar hello alkalmazás által igényelt összes hello függőségeket tartalmazó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="da685-153">hello maven-shade-plugin also produces an uber jar that contains all hello dependencies required by hello application.</span></span>

4. <span data-ttu-id="da685-154">Mentse a hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-154">Save hello `pom.xml` file.</span></span>

5. <span data-ttu-id="da685-155">Hozzon létre egy könyvtárat nevű `conf` a hello `hbaseapp` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="da685-155">Create a directory named `conf` in hello `hbaseapp` directory.</span></span> <span data-ttu-id="da685-156">Ebben a könyvtárban használt toohold tooHBase kapcsolódás konfigurációs adatait.</span><span class="sxs-lookup"><span data-stu-id="da685-156">This directory is used toohold configuration information for connecting tooHBase.</span></span>

6. <span data-ttu-id="da685-157">Használjon hello következő parancsot a toocopy hello HBase-konfiguráció hello HBase fürt toohello `conf` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="da685-157">Use hello following command toocopy hello HBase configuration from hello HBase cluster toohello `conf` directory.</span></span> <span data-ttu-id="da685-158">Cserélje le `USERNAME` hello nevet, az SSH-bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="da685-158">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="da685-159">Cserélje le `CLUSTERNAME` a HDInsight fürt nevű:</span><span class="sxs-lookup"><span data-stu-id="da685-159">Replace `CLUSTERNAME` with your HDInsight cluster name:</span></span>

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   <span data-ttu-id="da685-160">További információk az `ssh` és `scp`, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="da685-160">For more information on using `ssh` and `scp`, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="da685-161">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="da685-161">Create hello application</span></span>

1. <span data-ttu-id="da685-162">Nyissa meg toohello `hbaseapp/src/main/java/com/microsoft/examples` könyvtár, és nevezze át hello app.java fájl túl`CreateTable.java`.</span><span class="sxs-lookup"><span data-stu-id="da685-162">Go toohello `hbaseapp/src/main/java/com/microsoft/examples` directory and rename hello app.java file too`CreateTable.java`.</span></span>

2. <span data-ttu-id="da685-163">Nyissa meg hello `CreateTable.java` fájlt, és cserélje le a meglévő tartalom hello hello szöveg a következő:</span><span class="sxs-lookup"><span data-stu-id="da685-163">Open hello `CreateTable.java` file and replace hello existing contents with hello following text:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;
    import org.apache.hadoop.hbase.HTableDescriptor;
    import org.apache.hadoop.hbase.TableName;
    import org.apache.hadoop.hbase.HColumnDescriptor;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Put;
    import org.apache.hadoop.hbase.util.Bytes;

    public class CreateTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Example of setting zookeeper values for HDInsight
        // in code instead of an hbase-site.xml file
        //
        // config.set("hbase.zookeeper.quorum",
        //            "zookeepernode0,zookeepernode1,zookeepernode2");
        //config.set("hbase.zookeeper.property.clientPort", "2181");
        //config.set("hbase.cluster.distributed", "true");
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as hello znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using hello config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create hello table...
        HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
        // ... with two column families
        tableDescriptor.addFamily(new HColumnDescriptor("name"));
        tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
        admin.createTable(tableDescriptor);

        // define some people
        String[][] people = {
            { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
            { "2", "Franklin", "Holtz", "franklin@contoso.com" },
            { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
            { "4", "Rae", "Schroeder", "rae@contoso.com" },
            { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
            { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

        HTable table = new HTable(config, "people");

        // Add each person toohello table
        //   Use hello `name` column family for hello name
        //   Use hello `contactinfo` column family for hello email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close hello table
        table.flushCommits();
        table.close();
        }
    }
   ```

    <span data-ttu-id="da685-164">Ez a kód hello **CreateTable** osztályt, amely egy nevű táblát hoz létre **személyek** , és feltöltheti olyan előre definiált felhasználók.</span><span class="sxs-lookup"><span data-stu-id="da685-164">This code is hello **CreateTable** class, which creates a table named **people** and populate it with some predefined users.</span></span>

3. <span data-ttu-id="da685-165">Mentse a hello `CreateTable.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-165">Save hello `CreateTable.java` file.</span></span>

4. <span data-ttu-id="da685-166">A hello `hbaseapp/src/main/java/com/microsoft/examples` könyvtár, hozzon létre egy fájlt `SearchByEmail.java`.</span><span class="sxs-lookup"><span data-stu-id="da685-166">In hello `hbaseapp/src/main/java/com/microsoft/examples` directory, create a file named `SearchByEmail.java`.</span></span> <span data-ttu-id="da685-167">Szöveg hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="da685-167">Use hello following text as hello contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Scan;
    import org.apache.hadoop.hbase.client.ResultScanner;
    import org.apache.hadoop.hbase.client.Result;
    import org.apache.hadoop.hbase.filter.RegexStringComparator;
    import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
    import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
    import org.apache.hadoop.hbase.util.Bytes;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class SearchByEmail {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Use GenericOptionsParser tooget only hello parameters toohello class
        // and not all hello parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open hello table
        HTable table = new HTable(config, "people");

        // Define hello family and qualifiers toobe used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach hello regex filter tooa filter
        //   for hello email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set hello filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get hello results
        ResultScanner results = table.getScanner(scan);
        // Iterate over results and print  values
        for (Result result : results ) {
            String id = new String(result.getRow());
            byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
            String firstName = new String(firstNameObj);
            byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
            String lastName = new String(lastNameObj);
            System.out.println(firstName + " " + lastName + " - ID: " + id);
            byte[] emailObj = result.getValue(contactFamily, emailQualifier);
            String email = new String(emailObj);
            System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
        }
        results.close();
        table.close();
        }
    }
   ```

    <span data-ttu-id="da685-168">Hello **SearchByEmail** osztály használt tooquery sorok e-mail cím alapján lehet.</span><span class="sxs-lookup"><span data-stu-id="da685-168">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="da685-169">Mivel a program reguláris kifejezés szűrőt, megadhatja egy karakterlánc vagy egy reguláris kifejezést hello osztály használata esetén.</span><span class="sxs-lookup"><span data-stu-id="da685-169">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>

5. <span data-ttu-id="da685-170">Mentse a hello `SearchByEmail.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-170">Save hello `SearchByEmail.java` file.</span></span>

6. <span data-ttu-id="da685-171">A hello `hbaseapp/src/main/hava/com/microsoft/examples` könyvtár, hozzon létre egy fájlt `DeleteTable.java`.</span><span class="sxs-lookup"><span data-stu-id="da685-171">In hello `hbaseapp/src/main/hava/com/microsoft/examples` directory, create a file named `DeleteTable.java`.</span></span> <span data-ttu-id="da685-172">Szöveg hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="da685-172">Use hello following text as hello contents of this file:</span></span>

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using hello config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete hello table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    <span data-ttu-id="da685-173">Ez az osztály a szükségtelenné vált hello letiltásával ebben a példában létrehozott HBase táblákat és hello tábla eldobása hozta létre hello `CreateTable` osztály.</span><span class="sxs-lookup"><span data-stu-id="da685-173">This class cleans up hello HBase tables created in this example by disabling and dropping hello table created by hello `CreateTable` class.</span></span>

7. <span data-ttu-id="da685-174">Mentse a hello `DeleteTable.java` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-174">Save hello `DeleteTable.java` file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="da685-175">Buildelés és a csomag hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="da685-175">Build and package hello application</span></span>

1. <span data-ttu-id="da685-176">A hello `hbaseapp` könyvtárába, használjon hello következő parancsot a toobuild hello alkalmazást tartalmazó JAR-fájlt:</span><span class="sxs-lookup"><span data-stu-id="da685-176">From hello `hbaseapp` directory, use hello following command toobuild a JAR file that contains hello application:</span></span>

    ```bash
    mvn clean package
    ```

    <span data-ttu-id="da685-177">Ez a parancs létrehozza, és csomagok hello .jar fájl alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="da685-177">This command builds and packages hello application into a .jar file.</span></span>

2. <span data-ttu-id="da685-178">Hello parancs befejeződésekor hello `hbaseapp/target` directory nevű fájlt tartalmaz `hbaseapp-1.0-SNAPSHOT.jar`.</span><span class="sxs-lookup"><span data-stu-id="da685-178">When hello command completes, hello `hbaseapp/target` directory contains a file named `hbaseapp-1.0-SNAPSHOT.jar`.</span></span>

   > [!NOTE]
   > <span data-ttu-id="da685-179">Hello `hbaseapp-1.0-SNAPSHOT.jar` fájl egy uber jar.</span><span class="sxs-lookup"><span data-stu-id="da685-179">hello `hbaseapp-1.0-SNAPSHOT.jar` file is an uber jar.</span></span> <span data-ttu-id="da685-180">Minden hello függőségek szükséges toorun hello alkalmazást tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="da685-180">It contains all hello dependencies required toorun hello application.</span></span>


## <a name="upload-hello-jar-and-run-jobs-ssh"></a><span data-ttu-id="da685-181">Töltse fel a hello JAR-feladatok és futtatása (SSH)</span><span class="sxs-lookup"><span data-stu-id="da685-181">Upload hello JAR and run jobs (SSH)</span></span>

<span data-ttu-id="da685-182">a következő lépéseket használata hello `scp` toocopy hello JAR toohello elsődleges átjárócsomópontjához a HBase a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="da685-182">hello following steps use `scp` toocopy hello JAR toohello primary head node of your HBase on HDInsight cluster.</span></span> <span data-ttu-id="da685-183">Hello `ssh` parancs nem tooconnect toohello fürt használt, és futtassa közvetlenül hello példa hello átjárócsomópont.</span><span class="sxs-lookup"><span data-stu-id="da685-183">hello `ssh` command is then used tooconnect toohello cluster and run hello example directly on hello head node.</span></span>

1. <span data-ttu-id="da685-184">tooupload hello jar toohello fürt, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="da685-184">tooupload hello jar toohello cluster, use hello following command:</span></span>

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    <span data-ttu-id="da685-185">Cserélje le `USERNAME` hello nevet, az SSH-bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="da685-185">Replace `USERNAME` with hello name of your SSH login.</span></span> <span data-ttu-id="da685-186">Cserélje le `CLUSTERNAME` elemet a HDInsight fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="da685-186">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

2. <span data-ttu-id="da685-187">tooconnect toohello HBase-fürtöt, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="da685-187">tooconnect toohello HBase cluster, use hello following command:</span></span>

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    <span data-ttu-id="da685-188">Cserélje le `USERNAME` az SSH-bejelentkezéskor hello nevét.</span><span class="sxs-lookup"><span data-stu-id="da685-188">Replace `USERNAME` hello name of your SSH login.</span></span> <span data-ttu-id="da685-189">Cserélje le `CLUSTERNAME` elemet a HDInsight fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="da685-189">Replace `CLUSTERNAME` with your HDInsight cluster name.</span></span>

3. <span data-ttu-id="da685-190">egy HBase tábla használatával toocreate hello Java-alkalmazások, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="da685-190">toocreate an HBase table using hello Java application, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    <span data-ttu-id="da685-191">Ezzel a paranccsal létrejön egy HBase tábla nevű **személyek**, és tölti fel az adatokat.</span><span class="sxs-lookup"><span data-stu-id="da685-191">This command creates a HBase table named **people**, and populates it with data.</span></span>

4. <span data-ttu-id="da685-192">az e-mail címeket a következő parancs használata hello hello táblában tárolt toosearch:</span><span class="sxs-lookup"><span data-stu-id="da685-192">toosearch for email addresses stored in hello table, use hello following command:</span></span>

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    <span data-ttu-id="da685-193">A következő eredmények hello jelenhet meg:</span><span class="sxs-lookup"><span data-stu-id="da685-193">You receive hello following results:</span></span>

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. <span data-ttu-id="da685-194">toodelete hello tábla, a következő parancs használata hello:</span><span class="sxs-lookup"><span data-stu-id="da685-194">toodelete hello table, use hello following command:</span></span>

    

## <a name="upload-hello-jar-and-run-jobs-powershell"></a><span data-ttu-id="da685-195">Töltse fel a hello JAR-feladatok és futtatása (PowerShell)</span><span class="sxs-lookup"><span data-stu-id="da685-195">Upload hello JAR and run jobs (PowerShell)</span></span>

<span data-ttu-id="da685-196">a lépéseket követve hello Azure PowerShell tooupload hello JAR toohello alapértelmezett tárhelyet használja a HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="da685-196">hello following steps use Azure PowerShell tooupload hello JAR toohello default storage for your HBase cluster.</span></span> <span data-ttu-id="da685-197">HDInsight-parancsmagokat nem, akkor a használt toorun hello példák távolról.</span><span class="sxs-lookup"><span data-stu-id="da685-197">HDInsight cmdlets are then used toorun hello examples remotely.</span></span>

1. <span data-ttu-id="da685-198">Miután telepítése és konfigurálása az Azure PowerShell, hozzon létre egy fájlt `hbase-runner.psm1`.</span><span class="sxs-lookup"><span data-stu-id="da685-198">After installing and configuring Azure PowerShell, create a file named `hbase-runner.psm1`.</span></span> <span data-ttu-id="da685-199">Szöveg hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="da685-199">Use hello following text as hello contents of this file:</span></span>

   ```powershell
    <#
    .SYNOPSIS
    Copies a file toohello primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory toohello blob container for
    hello HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #hello class toorun
    [Parameter(Mandatory = $true)]
    [String]$className,

    #hello name of hello HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want toosee stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is hello Azure module installed?
    FindAzure

    # Get hello login for hello HDInsight cluster
    $creds=Get-Credential -Message "Enter hello login for hello cluster" -UserName "admin"

    # hello JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # hello job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get hello job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for hello job toocomplete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display hello standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file toohello primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory toohello blob container for
    hello HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #hello path toohello local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #hello destination path and file name, relative toohello root of hello container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #hello name of hello HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is hello Azure module installed?
        FindAzure

        # Get authentication for hello cluster
        $creds=Get-Credential

        # Does hello local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get hello primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file toostorage, overwriting existing files if -force was used.
        Set-AzureStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use hello Login-AzureRmAccount cmdlet toologin tooyour subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does hello cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get hello resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get hello storage context, as we can't depend
        # on using hello default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get hello container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts toosupport finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export hello verb-phrase things
    export-modulemember *-*
   ```

    <span data-ttu-id="da685-200">Ez a fájl két lehetővé tevő modulokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="da685-200">This file contains two modules:</span></span>

   * <span data-ttu-id="da685-201">**Adja hozzá HDInsightFile** - használt tooupload fájlok toohello fürt</span><span class="sxs-lookup"><span data-stu-id="da685-201">**Add-HDInsightFile** - used tooupload files toohello cluster</span></span>
   * <span data-ttu-id="da685-202">**Start-HBaseExample** -használt korábban létrehozott toorun hello osztályok</span><span class="sxs-lookup"><span data-stu-id="da685-202">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>

2. <span data-ttu-id="da685-203">Mentse a hello `hbase-runner.psm1` fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-203">Save hello `hbase-runner.psm1` file.</span></span>

3. <span data-ttu-id="da685-204">Nyisson meg egy új Azure PowerShell-ablakot, módosítsa a könyvtárakat toohello `hbaseapp` könyvtárat, majd futtatási hello végrehajtja a parancsot:</span><span class="sxs-lookup"><span data-stu-id="da685-204">Open a new Azure PowerShell window, change directories toohello `hbaseapp` directory, and then run hello following command:</span></span>

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    <span data-ttu-id="da685-205">Módosítani hello elérési toohello hello `hbase-runner.psm1` korábban létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="da685-205">Change hello path toohello location of hello `hbase-runner.psm1` file created earlier.</span></span> <span data-ttu-id="da685-206">Ez a parancs az Azure PowerShell hello modul regisztrálása.</span><span class="sxs-lookup"><span data-stu-id="da685-206">This command registers hello module with Azure PowerShell.</span></span>

4. <span data-ttu-id="da685-207">Használjon hello következő parancsot a tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour fürt.</span><span class="sxs-lookup"><span data-stu-id="da685-207">Use hello following command tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour cluster.</span></span>

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    <span data-ttu-id="da685-208">Cserélje le `hdinsightclustername` hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="da685-208">Replace `hdinsightclustername` with hello name of your cluster.</span></span> <span data-ttu-id="da685-209">hello parancs feltölt hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` hello elsődleges a fürt tárolóhelyét helyét.</span><span class="sxs-lookup"><span data-stu-id="da685-209">hello command uploads hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` location in hello primary storage for your cluster.</span></span>

5. <span data-ttu-id="da685-210">egy tábla használatával toocreate hello `hbaseapp`, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="da685-210">toocreate a table using hello `hbaseapp`, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    <span data-ttu-id="da685-211">Cserélje le `hdinsightclustername` hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="da685-211">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="da685-212">Ezzel a paranccsal létrejön nevű tábla **személyek** a HBase a HDInsight-fürtre.</span><span class="sxs-lookup"><span data-stu-id="da685-212">This command creates a table named **people** in HBase on your HDInsight cluster.</span></span> <span data-ttu-id="da685-213">Ez a parancs nem jeleníti meg a hello konzolablak kimenetet.</span><span class="sxs-lookup"><span data-stu-id="da685-213">This command does not show any output in hello console window.</span></span>

6. <span data-ttu-id="da685-214">a hello táblázatban, a következő parancs használata hello toosearch:</span><span class="sxs-lookup"><span data-stu-id="da685-214">toosearch for entries in hello table, use hello following command:</span></span>

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    <span data-ttu-id="da685-215">Cserélje le `hdinsightclustername` hello néven a fürt.</span><span class="sxs-lookup"><span data-stu-id="da685-215">Replace `hdinsightclustername` with hello name of your cluster.</span></span>

    <span data-ttu-id="da685-216">Ez a parancs hello `SearchByEmail` toosearch egyetlen sort sem az osztály adott hello `contactinformation` oszlop termékcsalád és hello `email` oszlopban hello karakterláncot tartalmaz `contoso.com`.</span><span class="sxs-lookup"><span data-stu-id="da685-216">This command uses hello `SearchByEmail` class toosearch for any rows where hello `contactinformation` column family and hello `email` column, contains hello string `contoso.com`.</span></span> <span data-ttu-id="da685-217">A következő eredmények hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="da685-217">You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="da685-218">Használatával **fabrikam.com** a hello `-emailRegex` értéket adja vissza hello felhasználóknak, akik számára **fabrikam.com** hello e-mail mezőben.</span><span class="sxs-lookup"><span data-stu-id="da685-218">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="da685-219">A reguláris kifejezések hello keresési kifejezés is használhatja.</span><span class="sxs-lookup"><span data-stu-id="da685-219">You can also use regular expressions as hello search term.</span></span> <span data-ttu-id="da685-220">Például **^ r** értéket ad vissza e-mail címek hello "r" betűvel kezdődik.</span><span class="sxs-lookup"><span data-stu-id="da685-220">For example, **^r** returns email addresses that begin with hello letter 'r'.</span></span>

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="da685-221">Nincs eredményeit, illetve a Start-HBaseExample használata váratlan eredményekhez</span><span class="sxs-lookup"><span data-stu-id="da685-221">No results or unexpected results when using Start-HBaseExample</span></span>

<span data-ttu-id="da685-222">Használjon hello `-showErr` paraméter tooview hello standard hiba (STDERR) futó hello feladat során előállított.</span><span class="sxs-lookup"><span data-stu-id="da685-222">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="da685-223">Hello tábla törlése</span><span class="sxs-lookup"><span data-stu-id="da685-223">Delete hello table</span></span>

<span data-ttu-id="da685-224">Amikor elkészült, a hello példa, használja a következő toodelete hello hello **személyek** ebben a példában használt tábla:</span><span class="sxs-lookup"><span data-stu-id="da685-224">When you are done with hello example, use hello following toodelete hello **people** table used in this example:</span></span>

<span data-ttu-id="da685-225">__Az egy `ssh` munkamenet__:</span><span class="sxs-lookup"><span data-stu-id="da685-225">__From an `ssh` session__:</span></span>

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

<span data-ttu-id="da685-226">__Az Azure PowerShell__:</span><span class="sxs-lookup"><span data-stu-id="da685-226">__From Azure PowerShell__:</span></span>

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a><span data-ttu-id="da685-227">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="da685-227">Next steps</span></span>

[<span data-ttu-id="da685-228">Megtudhatja, hogyan toouse SQuirreL SQL, a hbase eszközzel</span><span class="sxs-lookup"><span data-stu-id="da685-228">Learn how toouse SQuirreL SQL with HBase</span></span>](hdinsight-hbase-phoenix-squirrel-linux.md)
