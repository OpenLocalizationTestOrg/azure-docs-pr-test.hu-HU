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
# <a name="use-maven-toobuild-java-applications-that-use-hbase-with-windows-based-hdinsight-hadoop"></a>Maven toobuild Java-alkalmazások, amelyek használják a HBase és a Windows-alapú HDInsight (Hadoop) együttes használata
Megtudhatja, hogyan toocreate hozhat létre egy [Apache HBase](http://hbase.apache.org/) alkalmazás Apache Maven használatával Java nyelven. Majd használni hello alkalmazás Azure HDInsight (Hadoop).

[Maven](http://maven.apache.org/) szoftver project management és a szövegértést eszköz, amely lehetővé teszi a toobuild szoftver, a dokumentáció és a Java-projektek a jelentésekre. Ebből a cikkből megismerheti, hogyan toouse azt toocreate egy alapszintű Java-alkalmazást hoz létre, lekérdezi és törli a HBase tábla Azure HDInsight-fürtöt.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Windows igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Követelmények
* [Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 7 vagy újabb
* [Maven 3](http://maven.apache.org/)
* Egy Windows-alapú HDInsight-fürtöt, a hbase eszközzel

    > [!NOTE]
    > jelen dokumentumban leírt lépések hello HDInsight fürt verzióival 3.2-es és 3.3-as lettek tesztelve. hello alapértelmezett példákban megadott értékei HDInsight 3.3 fürt.

## <a name="create-hello-project"></a>Hello projekt létrehozása
1. Parancssorból hello a fejlesztési környezetben, módosítsa a könyvtárakat toohello helyének toocreate hello projekt, például `cd code\hdinsight`.
2. Használjon hello **mvn** parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.

        mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    Hello által megadott hello nevű ezzel a paranccsal létrejön az hello aktuális helyen, egy könyvtárat **artifactid szakaszát** paraméter (**hbaseapp** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:

   * **pom.xml**: hello projekt Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) konfigurációban használt toobuild hello projektet tartalmaz.
   * **src**: hello tartalmazó hello könyvtár **main\java\com\microsoft\examples** könyvtárban, ahol meg fog írni hello alkalmazás.
3. Törölje a hello **src\test\java\com\microsoft\examples\apptest.java** fájlhoz, mert nem szerepel ebben a példában.

## <a name="update-hello-project-object-model"></a>Frissítés hello projekt Hálózatiobjektum-modellje
1. Hello szerkesztése **pom.xml** fájlt, és adja hozzá a következő kódot hello hello `<dependencies>` szakasz:

        <dependency>
          <groupId>org.apache.hbase</groupId>
          <artifactId>hbase-client</artifactId>
          <version>1.1.2</version>
        </dependency>

    Ez a szakasz azt ismerteti, igényel, amely hello projektet Maven **hbase-ügyfél** verzió **1.1.2**. Fordítási időben a függőség letöltésének hello alapértelmezett Maven-tárházat. Használhatja a hello [Maven központi tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) további információk a függőség toolearn.

   > [!IMPORTANT]
   > hello verziószámnak egyeznie kell a HBase a HDInsight-fürthöz biztosított hello verzióját. A következő tábla toofind hello megfelelő verziószámot hello használata.
   >
   >

   | HDInsight-fürt verziószáma | A HBase verzió toouse |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3 |1.1.2 |

    A HDInsight-verziókról és összetevők további információkért lásd: [hello különböző Hadoop-összetevők és a HDInsight együttes rendelkezésre Mik](hdinsight-component-versioning.md).
2. Ha egy HDInsight 3.3-fürtöt használ, a következő toohello hello is hozzá kell adnia `<dependencies>` szakasz:

        <dependency>
            <groupId>org.apache.phoenix</groupId>
            <artifactId>phoenix-core</artifactId>
            <version>4.4.0-HBase-1.1</version>
        </dependency>

    A függőség betölti hello phoenix-alapvető összetevői, Hbase verziója által használt 1.1.x.
3. Adja hozzá a következő kód toohello hello **pom.xml** fájlt. Ez a szakasz hello belül kell `<project>...</project>` hello címkék fájlba, például közötti `</dependencies>` és `</project>`.

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

    Hello `<resources>` szakasz konfigurál egy erőforrást (**conf\hbase-site.xml**), amely a HBase konfigurációs információkat tartalmaz.

   > [!NOTE]
   > Megadhatja a konfigurációs értékeket keresztül kódot is. A hello hello megjegyzéseket **CreateTable** kapcsolatos alábbi példa toodo ez.
   >
   >

    Ez `<plugins>` szakasz hello konfigurálása [Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/). hello fordító beépülő modul használt toocompile hello topológia. hello árnyalatát beépülő modul használt tooprevent licenc ismétlődést hello JAR package Maven által épített. hello ez használható oka az, hogy hello ismétlődő licencfájlok hello HDInsight-fürt futás közben hibát okozhat. Maven-árnyalatát-beépülő modul használata hello `ApacheLicenseResourceTransformer` megvalósítási megakadályozza, hogy ez a hiba.

    hello maven-árnyalatát-beépülő modul is hoz létre egy uber jar (vagy az fat jar), amely tartalmazza az összes hello függőségek hello alkalmazáshoz szükséges.
4. Mentse a hello **pom.xml** fájlt.
5. Hozzon létre egy új könyvtárat nevű **conf** a hello **hbaseapp** könyvtár. A hello **conf** könyvtár, hozzon létre egy fájlt **hbase-site.xml**. Hello következő hello fájl tartalmának hello használata:

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

    Ez a fájl használt tooload hello HBase konfigurációs a HDInsight-fürtök lesz.

   > [!NOTE]
   > A minimális hbase-site.xml fájl, és operációs rendszer minimális beállítások hello hello HDInsight-fürtöt tartalmaz.

6. Mentse a hello **hbase-site.xml** fájlt.

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása
1. Nyissa meg toohello **hbaseapp\src\main\java\com\microsoft\examples** könyvtár, és nevezze át hello app.java fájl túl**CreateTable.java**.
2. Nyissa meg hello **CreateTable.java** fájlt, és cserélje le a meglévő tartalom hello hello a következő kódot:

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

    Ez a hello **CreateTable** osztályt, amely létrehoz egy táblát nevű **személyek** , és feltöltheti olyan előre definiált felhasználók.
3. Mentse a hello **CreateTable.java** fájlt.
4. A hello **hbaseapp\src\main\java\com\microsoft\examples** könyvtár, hozzon létre egy új fájlt **SearchByEmail.java**. Kód hello a fájl tartalmát, a következő hello használata:

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

    Hello **SearchByEmail** osztály használt tooquery sorok e-mail cím alapján lehet. Mivel a program reguláris kifejezés szűrőt, megadhatja egy karakterlánc vagy egy reguláris kifejezést hello osztály használata esetén.
5. Mentse a hello **SearchByEmail.java** fájlt.
6. A hello **hbaseapp\src\main\hava\com\microsoft\examples** könyvtár, hozzon létre egy új fájlt **DeleteTable.java**. Kód hello a fájl tartalmát, a következő hello használata:

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

    Ez az osztály van ebben a példában tisztítási letiltásával és hello tábla eldobása hozta létre hello **CreateTable** osztály.
7. Mentse a hello **DeleteTable.java** fájlt.

## <a name="build-and-package-hello-application"></a>Buildelés és a csomag hello alkalmazás
1. Nyisson meg egy parancssort, és módosítsa a könyvtárakat toohello **hbaseapp** könyvtár.
2. A következő parancs toobuild hello alkalmazást tartalmazó JAR-fájlra hello használata:

        mvn clean package

    Ez törli az előző build összetevőihez, letölti az összes függőséget, amely már nincs telepítve, majd épít fel és hello alkalmazás csomagok.
3. Hello parancs befejeződésekor hello **hbaseapp\target** directory nevű fájlt tartalmaz **hbaseapp-1.0-SNAPSHOT.jar**.

   > [!NOTE]
   > Hello **hbaseapp-1.0-SNAPSHOT.jar** fájl egy uber (más néven a fat jar) minden hello függőség tartalmazó jar toorun hello alkalmazás szükséges.

## <a name="upload-hello-jar-file-and-start-a-job"></a>Töltse fel a hello JAR-fájlra, és elindíthat egy feladatot
Nincsenek számos módon tooupload egy fájl tooyour HDInsight-fürtöt, a [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md). hello lépések Azure PowerShell használata.

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

1. Miután telepítése és konfigurálása az Azure PowerShell, hozzon létre egy új fájlt **hbase-runner.psm1**. A fájl tartalmának hello következő hello használata:

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

    Ez a fájl két lehetővé tevő modulokat tartalmaz:

   * **Adja hozzá HDInsightFile** -tooupload fájlok tooHDInsight használt
   * **Start-HBaseExample** -használt korábban létrehozott toorun hello osztályok
2. Mentse a hello **hbase-runner.psm1** fájlt.
3. Nyisson meg egy új Azure PowerShell-ablakot, módosítsa a könyvtárakat toohello **hbaseapp** könyvtárat, majd futtatási hello végrehajtja a parancsot.

        PS C:\ Import-Module c:\path\to\hbase-runner.psm1

    Módosítani hello elérési toohello hello **hbase-runner.psm1** korábban létrehozott fájlt. Hello modul ez regisztrál az Azure PowerShell-munkamenetben.
4. Használjon hello következő parancsot a tooupload hello **hbaseapp-1.0-SNAPSHOT.jar** tooyour HDInsight-fürthöz.

        Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername

    Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz. hello parancs feltölt hello **hbaseapp-1.0-SNAPSHOT.jar** toohello **példa/JAR-fájlok kivételével** hello a HDInsight-fürt elsődleges tároló helyét.
5. Miután hello fájlok feltöltése után használata hello következő kódot toocreate hello használó **hbaseapp**:

        Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername

    Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.

    Ezzel a paranccsal létrejön egy új tábla nevű **személyek** a HDInsight-fürtön. Ez a parancs nem jeleníti meg a hello konzolablak kimenetet.
6. a hello táblázatban, a következő parancs használata hello toosearch:

        Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com

    Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.

    Ez a parancs hello **SearchByEmail** toosearch egyetlen sort sem az osztály adott hello **contactinformation** oszlop termékcsalád és hello **e-mail** oszlopban hello karakterláncot tartalmaz **contoso.com**. A következő eredmények hello kell megjelennie:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Használatával **fabrikam.com** a hello `-emailRegex` értéket adja vissza hello felhasználóknak, akik számára **fabrikam.com** hello e-mail mezőben. Ez a keresés reguláris kifejezés alapú szűrő használatával valósul meg, mert is megadhat reguláris kifejezéseket, például a **^ r**, mely értéket ad vissza bejegyzéseket, ahol az üdvözlő e-mail hello levél "r" kezdődik.

## <a name="delete-hello-table"></a>Hello tábla törlése
Amikor elkészült, a hello példa, használjon hello következő parancsot a hello Azure PowerShell-munkamenet toodelete hello a **személyek** ebben a példában használt tábla:

    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername

Cserélje le **hdinsightclustername** hello nevet, a HDInsight-fürthöz.

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Nincs eredményeit, illetve a Start-HBaseExample használata váratlan eredményekhez
Használjon hello `-showErr` paraméter tooview hello standard hiba (STDERR) futó hello feladat során előállított.
