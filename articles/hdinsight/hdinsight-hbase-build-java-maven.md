---
title: "egy Windows-alapú Azure HDInsight Java HBase-alkalmazást aaaBuild |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Apache Maven Java-alapú toobuild Apache HBase alkalmazást, majd telepítenie kell azt tooa Windows-alapú Azure HDInsight-fürtöt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7f4a4e02-45ab-40dd-842b-3ec034f256c9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/05/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 33c2f3d12cb6a17b5406817e8bcd3accff239517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a><span data-ttu-id="6921a-103">Maven toobuild Java-alkalmazások, amelyek használják a HBase és a Windows-alapú HDInsight (Hadoop) együttes használata</span><span class="sxs-lookup"><span data-stu-id="6921a-103">Use Maven toobuild Java applications that use HBase with Windows-based HDInsight (Hadoop)</span></span>
<span data-ttu-id="6921a-104">Megtudhatja, hogyan toocreate hozhat létre egy [Apache HBase](http://hbase.apache.org/) alkalmazás Apache Maven használatával Java nyelven.</span><span class="sxs-lookup"><span data-stu-id="6921a-104">Learn how toocreate and build an [Apache HBase](http://hbase.apache.org/) application in Java by using Apache Maven.</span></span> <span data-ttu-id="6921a-105">Majd használni hello alkalmazás Azure HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="6921a-105">Then use hello application with Azure HDInsight (Hadoop).</span></span>

<span data-ttu-id="6921a-106">[Maven](http://maven.apache.org/) szoftver project management és a szövegértést eszköz, amely lehetővé teszi a toobuild szoftver, a dokumentáció és a Java-projektek a jelentésekre.</span><span class="sxs-lookup"><span data-stu-id="6921a-106">[Maven](http://maven.apache.org/) is a software project management and comprehension tool that allows you toobuild software, documentation, and reports for Java projects.</span></span> <span data-ttu-id="6921a-107">Ebből a cikkből megismerheti, hogyan toouse azt toocreate egy alapszintű Java-alkalmazást hoz létre, lekérdezi és törli a HBase tábla Azure HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="6921a-107">In this article, you learn how toouse it toocreate a basic Java application that that creates, queries, and deletes an HBase table on an Azure HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6921a-108">hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Windows igényelnek.</span><span class="sxs-lookup"><span data-stu-id="6921a-108">hello steps in this document require an HDInsight cluster that uses Windows.</span></span> <span data-ttu-id="6921a-109">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="6921a-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6921a-110">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6921a-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

## <a name="requirements"></a><span data-ttu-id="6921a-111">Követelmények</span><span class="sxs-lookup"><span data-stu-id="6921a-111">Requirements</span></span>
* <span data-ttu-id="6921a-112">[Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 vagy újabb</span><span class="sxs-lookup"><span data-stu-id="6921a-112">[Java platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 or later</span></span>
* [<span data-ttu-id="6921a-113">Maven 3</span><span class="sxs-lookup"><span data-stu-id="6921a-113">Maven</span></span>](http://maven.apache.org/)
* <span data-ttu-id="6921a-114">Egy Windows-alapú HDInsight-fürtöt, a hbase eszközzel</span><span class="sxs-lookup"><span data-stu-id="6921a-114">A Windows-based HDInsight cluster with HBase</span></span>

    > [!NOTE]
    > <span data-ttu-id="6921a-115">jelen dokumentumban leírt lépések hello HDInsight fürt verzióival 3.2-es és 3.3-as lettek tesztelve.</span><span class="sxs-lookup"><span data-stu-id="6921a-115">hello steps in this document have been tested with HDInsight cluster versions 3.2 and 3.3.</span></span> <span data-ttu-id="6921a-116">hello alapértelmezett példákban megadott értékei HDInsight 3.3 fürt.</span><span class="sxs-lookup"><span data-stu-id="6921a-116">hello default values provided in examples are for a HDInsight 3.3 cluster.</span></span>

## <a name="create-hello-project"></a><span data-ttu-id="6921a-117">Hello projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6921a-117">Create hello project</span></span>
1. <span data-ttu-id="6921a-118">Parancssorból hello a fejlesztési környezetben, módosítsa a könyvtárakat toohello helyének toocreate hello projekt, például `cd code\hdinsight`.</span><span class="sxs-lookup"><span data-stu-id="6921a-118">From hello command line in your development environment, change directories toohello location where you want toocreate hello project, for example, `cd code\hdinsight`.</span></span>
2. <span data-ttu-id="6921a-119">Használjon hello **mvn** parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.</span><span class="sxs-lookup"><span data-stu-id="6921a-119">Use hello **mvn** command, which is installed with Maven, toogenerate hello scaffolding for hello project.</span></span>

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    <span data-ttu-id="6921a-120">Hello által megadott hello nevű ezzel a paranccsal létrejön az hello aktuális helyen, egy könyvtárat **artifactid szakaszát** paraméter (**hbaseapp** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="6921a-120">This command creates a directory in hello current location, with hello name specified by hello **artifactID** parameter (**hbaseapp** in this example.) This directory contains hello following items:</span></span>

   * <span data-ttu-id="6921a-121">**pom.xml**: hello projekt Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) konfigurációban használt toobuild hello projektet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6921a-121">**pom.xml**:  hello Project Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) contains information and configuration details used toobuild hello project.</span></span>
   * <span data-ttu-id="6921a-122">**src**: hello tartalmazó hello könyvtár **main\java\com\microsoft\examples** könyvtárban, ahol meg fog írni hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6921a-122">**src**: hello directory that contains hello **main\java\com\microsoft\examples** directory, where you will author hello application.</span></span>
3. <span data-ttu-id="6921a-123">Törölje a hello **src\test\java\com\microsoft\examples\apptest.java** fájlhoz, mert nem szerepel ebben a példában.</span><span class="sxs-lookup"><span data-stu-id="6921a-123">Delete hello **src\test\java\com\microsoft\examples\apptest.java** file because it is not used in this example.</span></span>

## <a name="update-hello-project-object-model"></a><span data-ttu-id="6921a-124">Frissítés hello projekt Hálózatiobjektum-modellje</span><span class="sxs-lookup"><span data-stu-id="6921a-124">Update hello Project Object Model</span></span>
1. <span data-ttu-id="6921a-125">Hello szerkesztése **pom.xml** fájlt, és adja hozzá a következő kódot hello hello `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="6921a-125">Edit hello **pom.xml** file and add hello following code inside hello `<dependencies>` section:</span></span>

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    <span data-ttu-id="6921a-126">Ez a szakasz azt ismerteti, igényel, amely hello projektet Maven **hbase-ügyfél** verzió **1.1.2**.</span><span class="sxs-lookup"><span data-stu-id="6921a-126">This section tells Maven that hello project requires **hbase-client** version **1.1.2**.</span></span> <span data-ttu-id="6921a-127">Fordítási időben a függőség letöltésének hello alapértelmezett Maven-tárházat.</span><span class="sxs-lookup"><span data-stu-id="6921a-127">At compile time, this dependency is downloaded from hello default Maven repository.</span></span> <span data-ttu-id="6921a-128">Használhatja a hello [Maven központi tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) további információk a függőség toolearn.</span><span class="sxs-lookup"><span data-stu-id="6921a-128">You can use hello [Maven Central Repository Search](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) toolearn more about this dependency.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="6921a-129">hello verziószámnak egyeznie kell a HBase a HDInsight-fürthöz biztosított hello verzióját.</span><span class="sxs-lookup"><span data-stu-id="6921a-129">hello version number must match hello version of HBase that is provided with your HDInsight cluster.</span></span> <span data-ttu-id="6921a-130">A következő tábla toofind hello megfelelő verziószámot hello használata.</span><span class="sxs-lookup"><span data-stu-id="6921a-130">Use hello following table toofind hello correct version number.</span></span>
   >
   >

   | <span data-ttu-id="6921a-131">HDInsight-fürt verziószáma</span><span class="sxs-lookup"><span data-stu-id="6921a-131">HDInsight cluster version</span></span> | <span data-ttu-id="6921a-132">A HBase verzió toouse</span><span class="sxs-lookup"><span data-stu-id="6921a-132">HBase version toouse</span></span> |
   | --- | --- |
   | <span data-ttu-id="6921a-133">3.2</span><span class="sxs-lookup"><span data-stu-id="6921a-133">3.2</span></span> |<span data-ttu-id="6921a-134">0.98.4-hadoop2</span><span class="sxs-lookup"><span data-stu-id="6921a-134">0.98.4-hadoop2</span></span> |
   | <span data-ttu-id="6921a-135">3.3</span><span class="sxs-lookup"><span data-stu-id="6921a-135">3.3</span></span> |<span data-ttu-id="6921a-136">1.1.2</span><span class="sxs-lookup"><span data-stu-id="6921a-136">1.1.2</span></span> |

    <span data-ttu-id="6921a-137">A HDInsight-verziókról és összetevők további információkért lásd: [hello különböző Hadoop-összetevők és a HDInsight együttes rendelkezésre Mik](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="6921a-137">For more information on HDInsight versions and components, see [What are hello different Hadoop components available with HDInsight](hdinsight-component-versioning.md).</span></span>
2. <span data-ttu-id="6921a-138">Ha egy HDInsight 3.3-fürtöt használ, a következő toohello hello is hozzá kell adnia `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="6921a-138">If you are using an HDInsight 3.3 cluster, you must also add hello following toohello `<dependencies>` section:</span></span>

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    <span data-ttu-id="6921a-139">A függőség betölti hello phoenix-alapvető összetevői, Hbase verziója által használt 1.1.x.</span><span class="sxs-lookup"><span data-stu-id="6921a-139">This dependency will load hello phoenix-core components, which are used by Hbase version 1.1.x.</span></span>
3. <span data-ttu-id="6921a-140">Adja hozzá a következő kód toohello hello **pom.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-140">Add hello following code toohello **pom.xml** file.</span></span> <span data-ttu-id="6921a-141">Ez a szakasz hello belül kell `<project>...</project>` hello címkék fájlba, például közötti `</dependencies>` és `</project>`.</span><span class="sxs-lookup"><span data-stu-id="6921a-141">This section must be inside hello `<project>...</project>` tags in hello file, for example, between `</dependencies>` and `</project>`.</span></span>

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
                    <source>1.7</source>
                    <target>1.7</target>
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

    <span data-ttu-id="6921a-142">Hello `<resources>` szakasz konfigurál egy erőforrást (**conf\hbase-site.xml**), amely a HBase konfigurációs információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6921a-142">hello `<resources>` section configures a resource (**conf\hbase-site.xml**) that contains configuration information for HBase.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6921a-143">Megadhatja a konfigurációs értékeket keresztül kódot is.</span><span class="sxs-lookup"><span data-stu-id="6921a-143">You can also set configuration values via code.</span></span> <span data-ttu-id="6921a-144">A hello hello megjegyzéseket **CreateTable** kapcsolatos alábbi példa toodo ez.</span><span class="sxs-lookup"><span data-stu-id="6921a-144">See hello comments in hello **CreateTable** example that follows for how toodo this.</span></span>
   >
   >

    <span data-ttu-id="6921a-145">Ez `<plugins>` szakasz hello konfigurálása [Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/).</span><span class="sxs-lookup"><span data-stu-id="6921a-145">This `<plugins>` section configures hello [Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/) and [Maven Shade Plugin](http://maven.apache.org/plugins/maven-shade-plugin/).</span></span> <span data-ttu-id="6921a-146">hello fordító beépülő modul használt toocompile hello topológia.</span><span class="sxs-lookup"><span data-stu-id="6921a-146">hello compiler plug-in is used toocompile hello topology.</span></span> <span data-ttu-id="6921a-147">hello árnyalatát beépülő modul használt tooprevent licenc ismétlődést hello JAR package Maven által épített.</span><span class="sxs-lookup"><span data-stu-id="6921a-147">hello shade plug-in is used tooprevent license duplication in hello JAR package that is built by Maven.</span></span> <span data-ttu-id="6921a-148">hello ez használható oka az, hogy hello ismétlődő licencfájlok hello HDInsight-fürt futás közben hibát okozhat.</span><span class="sxs-lookup"><span data-stu-id="6921a-148">hello reason this is used is that hello duplicate license files cause an error at run time on hello HDInsight cluster.</span></span> <span data-ttu-id="6921a-149">Maven-árnyalatát-beépülő modul használata hello `ApacheLicenseResourceTransformer` megvalósítási megakadályozza, hogy ez a hiba.</span><span class="sxs-lookup"><span data-stu-id="6921a-149">Using maven-shade-plugin with hello `ApacheLicenseResourceTransformer` implementation prevents this error.</span></span>

    <span data-ttu-id="6921a-150">hello maven-árnyalatát-beépülő modul is hoz létre egy uber jar (vagy az fat jar), amely tartalmazza az összes hello függőségek hello alkalmazáshoz szükséges.</span><span class="sxs-lookup"><span data-stu-id="6921a-150">hello maven-shade-plugin also produces an uber jar (or fat jar) that contains all hello dependencies required by hello application.</span></span>
4. <span data-ttu-id="6921a-151">Mentse a hello **pom.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-151">Save hello **pom.xml** file.</span></span>
5. <span data-ttu-id="6921a-152">Hozzon létre egy új könyvtárat nevű **conf** a hello **hbaseapp** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6921a-152">Create a new directory named **conf** in hello **hbaseapp** directory.</span></span> <span data-ttu-id="6921a-153">A hello **conf** könyvtár, hozzon létre egy fájlt **hbase-site.xml**.</span><span class="sxs-lookup"><span data-stu-id="6921a-153">In hello **conf** directory, create a file named **hbase-site.xml**.</span></span> <span data-ttu-id="6921a-154">Hello következő hello fájl tartalmának hello használata:</span><span class="sxs-lookup"><span data-stu-id="6921a-154">Use hello following as hello contents of hello file:</span></span>

        <?xml version="1.0"?>
        <?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
        <!--
        /**
          * Copyright 2010 hello Apache Software Foundation
          *
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
        -->
        <configuration>
          <property>
            <name>hbase.cluster.distributed</name>
            <value>true</value>
          </property>
          <property>
            <name>hbase.zookeeper.quorum</name>
            <value>zookeeper0,zookeeper1,zookeeper2</value>
          </property>
          <property>
            <name>hbase.zookeeper.property.clientPort</name>
            <value>2181</value>
          </property>
        </configuration>

    <span data-ttu-id="6921a-155">Ez a fájl használt tooload hello HBase konfigurációs a HDInsight-fürtök lesz.</span><span class="sxs-lookup"><span data-stu-id="6921a-155">This file will be used tooload hello HBase configuration for an HDInsight cluster.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6921a-156">A minimális hbase-site.xml fájl, és operációs rendszer minimális beállítások hello hello HDInsight-fürtöt tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6921a-156">This is a minimal hbase-site.xml file, and it contains hello bare minimum settings for hello HDInsight cluster.</span></span>

6. <span data-ttu-id="6921a-157">Mentse a hello **hbase-site.xml** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-157">Save hello **hbase-site.xml** file.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="6921a-158">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6921a-158">Create hello application</span></span>
1. <span data-ttu-id="6921a-159">Nyissa meg toohello **hbaseapp\src\main\java\com\microsoft\examples** könyvtár, és nevezze át hello app.java fájl túl**CreateTable.java**.</span><span class="sxs-lookup"><span data-stu-id="6921a-159">Go toohello **hbaseapp\src\main\java\com\microsoft\examples** directory and rename hello app.java file too**CreateTable.java**.</span></span>
2. <span data-ttu-id="6921a-160">Nyissa meg hello **CreateTable.java** fájlt, és cserélje le a meglévő tartalom hello hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="6921a-160">Open hello **CreateTable.java** file and replace hello existing contents with hello following code:</span></span>

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
            // hello following sets hello znode root for Linux-based HDInsight
            //config.set("zookeeper.znode.parent","/hbase-unsecure");

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

    <span data-ttu-id="6921a-161">Ez a hello **CreateTable** osztályt, amely létrehoz egy táblát nevű **személyek** , és feltöltheti olyan előre definiált felhasználók.</span><span class="sxs-lookup"><span data-stu-id="6921a-161">This is hello **CreateTable** class, which will create a table named **people** and populate it with some predefined users.</span></span>
3. <span data-ttu-id="6921a-162">Mentse a hello **CreateTable.java** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-162">Save hello **CreateTable.java** file.</span></span>
4. <span data-ttu-id="6921a-163">A hello **hbaseapp\src\main\java\com\microsoft\examples** könyvtár, hozzon létre egy új fájlt **SearchByEmail.java**.</span><span class="sxs-lookup"><span data-stu-id="6921a-163">In hello **hbaseapp\src\main\java\com\microsoft\examples** directory, create a new file named **SearchByEmail.java**.</span></span> <span data-ttu-id="6921a-164">Kód hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="6921a-164">Use hello following code as hello contents of this file:</span></span>

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

            // Create a new regex filter
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

    <span data-ttu-id="6921a-165">Hello **SearchByEmail** osztály használt tooquery sorok e-mail cím alapján lehet.</span><span class="sxs-lookup"><span data-stu-id="6921a-165">hello **SearchByEmail** class can be used tooquery for rows by email address.</span></span> <span data-ttu-id="6921a-166">Mivel a program reguláris kifejezés szűrőt, megadhatja egy karakterlánc vagy egy reguláris kifejezést hello osztály használata esetén.</span><span class="sxs-lookup"><span data-stu-id="6921a-166">Because it uses a regular expression filter, you can provide either a string or a regular expression when using hello class.</span></span>
5. <span data-ttu-id="6921a-167">Mentse a hello **SearchByEmail.java** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-167">Save hello **SearchByEmail.java** file.</span></span>
6. <span data-ttu-id="6921a-168">A hello **hbaseapp\src\main\hava\com\microsoft\examples** könyvtár, hozzon létre egy új fájlt **DeleteTable.java**.</span><span class="sxs-lookup"><span data-stu-id="6921a-168">In hello **hbaseapp\src\main\hava\com\microsoft\examples** directory, create a new file named **DeleteTable.java**.</span></span> <span data-ttu-id="6921a-169">Kód hello a fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="6921a-169">Use hello following code as hello contents of this file:</span></span>

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

    <span data-ttu-id="6921a-170">Ez az osztály van ebben a példában tisztítási letiltásával és hello tábla eldobása hozta létre hello **CreateTable** osztály.</span><span class="sxs-lookup"><span data-stu-id="6921a-170">This class is for cleaning up this example by disabling and dropping hello table created by hello **CreateTable** class.</span></span>
7. <span data-ttu-id="6921a-171">Mentse a hello **DeleteTable.java** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-171">Save hello **DeleteTable.java** file.</span></span>

## <a name="build-and-package-hello-application"></a><span data-ttu-id="6921a-172">Buildelés és a csomag hello alkalmazás</span><span class="sxs-lookup"><span data-stu-id="6921a-172">Build and package hello application</span></span>
1. <span data-ttu-id="6921a-173">Nyisson meg egy parancssort, és módosítsa a könyvtárakat toohello **hbaseapp** könyvtár.</span><span class="sxs-lookup"><span data-stu-id="6921a-173">Open a command prompt and change directories toohello **hbaseapp** directory.</span></span>
2. <span data-ttu-id="6921a-174">A következő parancs toobuild hello alkalmazást tartalmazó JAR-fájlra hello használata:</span><span class="sxs-lookup"><span data-stu-id="6921a-174">Use hello following command toobuild a JAR file that contains hello application:</span></span>

        mvn clean package

    <span data-ttu-id="6921a-175">Ez törli az előző build összetevőihez, letölti az összes függőséget, amely már nincs telepítve, majd épít fel és hello alkalmazás csomagok.</span><span class="sxs-lookup"><span data-stu-id="6921a-175">This cleans any previous build artifacts, downloads any dependencies that have not already been installed, then builds and packages hello application.</span></span>
3. <span data-ttu-id="6921a-176">Hello parancs befejeződésekor hello **hbaseapp\target** directory nevű fájlt tartalmaz **hbaseapp-1.0-SNAPSHOT.jar**.</span><span class="sxs-lookup"><span data-stu-id="6921a-176">When hello command completes, hello **hbaseapp\target** directory contains a file named **hbaseapp-1.0-SNAPSHOT.jar**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="6921a-177">Hello **hbaseapp-1.0-SNAPSHOT.jar** fájl egy uber (más néven a fat jar) minden hello függőség tartalmazó jar toorun hello alkalmazás szükséges.</span><span class="sxs-lookup"><span data-stu-id="6921a-177">hello **hbaseapp-1.0-SNAPSHOT.jar** file is an uber jar (sometimes called a fat jar,) which contains all hello dependencies required toorun hello application.</span></span>

## <a name="upload-hello-jar-file-and-start-a-job"></a><span data-ttu-id="6921a-178">Töltse fel a hello JAR-fájlra, és elindíthat egy feladatot</span><span class="sxs-lookup"><span data-stu-id="6921a-178">Upload hello JAR file and start a job</span></span>
<span data-ttu-id="6921a-179">Nincsenek számos módon tooupload egy fájl tooyour HDInsight-fürtöt, a [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="6921a-179">There are many ways tooupload a file tooyour HDInsight cluster, as described in [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span> <span data-ttu-id="6921a-180">hello lépések Azure PowerShell használata.</span><span class="sxs-lookup"><span data-stu-id="6921a-180">hello following steps use Azure PowerShell.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. <span data-ttu-id="6921a-181">Miután telepítése és konfigurálása az Azure PowerShell, hozzon létre egy új fájlt **hbase-runner.psm1**.</span><span class="sxs-lookup"><span data-stu-id="6921a-181">After installing and configuring Azure PowerShell, create a new file named **hbase-runner.psm1**.</span></span> <span data-ttu-id="6921a-182">A fájl tartalmának hello következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="6921a-182">Use hello following as hello contents of this file:</span></span>

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

    <span data-ttu-id="6921a-183">Ez a fájl két lehetővé tevő modulokat tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="6921a-183">This file contains two modules:</span></span>

   * <span data-ttu-id="6921a-184">**Adja hozzá HDInsightFile** -tooupload fájlok tooHDInsight használt</span><span class="sxs-lookup"><span data-stu-id="6921a-184">**Add-HDInsightFile** - used tooupload files tooHDInsight</span></span>
   * <span data-ttu-id="6921a-185">**Start-HBaseExample** -használt korábban létrehozott toorun hello osztályok</span><span class="sxs-lookup"><span data-stu-id="6921a-185">**Start-HBaseExample** - used toorun hello classes created earlier</span></span>
2. <span data-ttu-id="6921a-186">Mentse a hello **hbase-runner.psm1** fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-186">Save hello **hbase-runner.psm1** file.</span></span>
3. <span data-ttu-id="6921a-187">Nyisson meg egy új Azure PowerShell-ablakot, módosítsa a könyvtárakat toohello **hbaseapp** könyvtárat, majd futtatási hello végrehajtja a parancsot.</span><span class="sxs-lookup"><span data-stu-id="6921a-187">Open a new Azure PowerShell window, change directories toohello **hbaseapp** directory, and then run hello following command.</span></span>

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    <span data-ttu-id="6921a-188">Módosítani hello elérési toohello hello **hbase-runner.psm1** korábban létrehozott fájlt.</span><span class="sxs-lookup"><span data-stu-id="6921a-188">Change hello path toohello location of hello **hbase-runner.psm1** file created earlier.</span></span> <span data-ttu-id="6921a-189">Hello modul ez regisztrál az Azure PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="6921a-189">This registers hello module for this Azure PowerShell session.</span></span>
4. <span data-ttu-id="6921a-190">Használjon hello következő parancsot a tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6921a-190">Use hello following command tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight cluster.</span></span>

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    <span data-ttu-id="6921a-191">Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6921a-191">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span> <span data-ttu-id="6921a-192">hello parancs feltölt hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **példa/JAR-fájlok kivételével** hello a HDInsight-fürt elsődleges tároló helyét.</span><span class="sxs-lookup"><span data-stu-id="6921a-192">hello command uploads hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **example/jars** location in hello primary storage for your HDInsight cluster.</span></span>
5. <span data-ttu-id="6921a-193">Miután hello fájlok feltöltése után használata hello következő kódot toocreate hello használó **hbaseapp**:</span><span class="sxs-lookup"><span data-stu-id="6921a-193">After hello files are uploaded, use hello following code toocreate a table using hello **hbaseapp**:</span></span>

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    <span data-ttu-id="6921a-194">Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6921a-194">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="6921a-195">Ezzel a paranccsal létrejön egy új tábla nevű **személyek** a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="6921a-195">This command creates a new table named **people** in your HDInsight cluster.</span></span> <span data-ttu-id="6921a-196">Ez a parancs nem jeleníti meg a hello konzolablak kimenetet.</span><span class="sxs-lookup"><span data-stu-id="6921a-196">This command does not show any output in hello console window.</span></span>
6. <span data-ttu-id="6921a-197">a hello táblázatban, a következő parancs használata hello toosearch:</span><span class="sxs-lookup"><span data-stu-id="6921a-197">toosearch for entries in hello table, use hello following command:</span></span>

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    <span data-ttu-id="6921a-198">Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6921a-198">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="6921a-199">Ez a parancs hello **SearchByEmail** toosearch egyetlen sort sem az osztály adott hello **contactinformation** oszlop termékcsalád és hello **e-mail** oszlopban hello karakterláncot tartalmaz **contoso.com**. A következő eredmények hello kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="6921a-199">This command uses hello **SearchByEmail** class toosearch for any rows where hello **contactinformation** column family and hello **email** column, contains hello string **contoso.com**. You should receive hello following results:</span></span>

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    <span data-ttu-id="6921a-200">Használatával **fabrikam.com** a hello `-emailRegex` értéket adja vissza hello felhasználóknak, akik számára **fabrikam.com** hello e-mail mezőben.</span><span class="sxs-lookup"><span data-stu-id="6921a-200">Using **fabrikam.com** for hello `-emailRegex` value returns hello users that have **fabrikam.com** in hello email field.</span></span> <span data-ttu-id="6921a-201">Ez a keresés reguláris kifejezés alapú szűrő használatával valósul meg, mert is megadhat reguláris kifejezéseket, például a **^ r**, mely értéket ad vissza bejegyzéseket, ahol az üdvözlő e-mail hello levél "r" kezdődik.</span><span class="sxs-lookup"><span data-stu-id="6921a-201">Since this search is implemented by using a regular expression-based filter, you can also enter regular expressions, such as **^r**, which returns entries where hello email begins with hello letter 'r'.</span></span>

## <a name="delete-hello-table"></a><span data-ttu-id="6921a-202">Hello tábla törlése</span><span class="sxs-lookup"><span data-stu-id="6921a-202">Delete hello table</span></span>
<span data-ttu-id="6921a-203">Amikor elkészült, a hello példa, használjon hello következő parancsot a hello Azure PowerShell-munkamenet toodelete hello a **személyek** ebben a példában használt tábla:</span><span class="sxs-lookup"><span data-stu-id="6921a-203">When you are done with hello example, use hello following command from hello Azure PowerShell session toodelete hello **people** table used in this example:</span></span>

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

<span data-ttu-id="6921a-204">Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="6921a-204">Replace **hdinsightclustername** with hello name of your HDInsight cluster.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="6921a-205">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6921a-205">Troubleshooting</span></span>
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a><span data-ttu-id="6921a-206">Nincs eredményeit, illetve a Start-HBaseExample használata váratlan eredményekhez</span><span class="sxs-lookup"><span data-stu-id="6921a-206">No results or unexpected results when using Start-HBaseExample</span></span>
<span data-ttu-id="6921a-207">Használjon hello `-showErr` paraméter tooview hello standard hiba (STDERR) futó hello feladat során előállított.</span><span class="sxs-lookup"><span data-stu-id="6921a-207">Use hello `-showErr` parameter tooview hello standard error (STDERR) that is produced while running hello job.</span></span>
