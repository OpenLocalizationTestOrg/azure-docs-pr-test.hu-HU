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
# <a name="build-java-applications-for-apache-hbase"></a>Az Apache HBase Java-alkalmazások létrehozása

Megtudhatja, hogyan toocreate egy [Apache HBase](http://hbase.apache.org/) alkalmazás Java nyelven. Majd használni az Azure HDInsight HBase hello alkalmazás.

Ez a dokumentum használatát a lépések hello [Maven](http://maven.apache.org/) toocreate és -buildek hello projekt. Maven egy a projekt szoftverkezelés és toobuild szoftver, dokumentáció és jelentések Java-projektek esetében érthetőséget eszközzel.

> [!NOTE]
> hello jelen dokumentumban leírt lépések alapján legutóbb történő teszteléskor az HDInsight 3.6.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Linux igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="requirements"></a>Követelmények

* [Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 vagy újabb.

    > [!NOTE]
    > HDInsight 3.5-ös és nagyobb Java 8 igényel. HDInsight a korábbi Java 7 igényelnek.

* [Maven 3](http://maven.apache.org/)

* [A HBase Linux-alapú Azure HDInsight fürt](hdinsight-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

  > [!NOTE]
  > jelen dokumentumban leírt lépések hello HDInsight fürt 3.4 és verziójú 3.5 lettek tesztelve. hello alapértelmezett példákban megadott értékei HDInsight 3.5 fürt.

## <a name="create-hello-project"></a>Hello projekt létrehozása

1. Parancssorból hello a fejlesztési környezetben, módosítsa a könyvtárakat toohello helyének toocreate hello projekt, például `cd code\hbase`.

2. Használjon hello **mvn** parancsot, amely Maven, hello projekt szerkezetet toogenerate hello együtt települ.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > Ha a PowerShell használata esetén hello közé kell `-D` az idézőjelek közé foglalt paraméterek.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Ez a parancs létrehoz egy könyvtárat az azonos nevet, amint hello hello **artifactid szakaszát** paraméter (**hbaseapp** ebben a példában.) Ez a könyvtár hello a következő elemeket tartalmazza:

   * **pom.xml**: hello projekt Object Model ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) konfigurációban használt toobuild hello projektet tartalmaz.
   * **src**: hello tartalmazó hello könyvtár **main/java/com/microsoft/példák** könyvtár, amikor szerzőként hello alkalmazás.

3. Törölje a hello `src/test/java/com/microsoft/examples/apptest.java` fájlt. Nem használható ebben a példában.

## <a name="update-hello-project-object-model"></a>Frissítés hello projekt Hálózatiobjektum-modellje

1. Hello szerkesztése `pom.xml` fájlt, és adja hozzá a következő kódot hello hello `<dependencies>` szakasz:

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

    Ez a szakasz azt jelzi, hogy hello projekt be kell **hbase-ügyfél** és **phoenix-core** összetevőket. Fordítási időben a függőségek hello alapértelmezett Maven tárházból lesznek letöltve. Használhatja a hello [Maven központi tárház keresési](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) további információk a függőség toolearn.

   > [!IMPORTANT]
   > hello hbase-ügyfél verziószáma hello egyeznie kell a HBase a HDInsight-fürthöz biztosított hello verziójával. A következő tábla toofind hello megfelelő verziószámot hello használata.

   | HDInsight-fürt verziószáma | A HBase verzió toouse |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3-as, 3.4, 3.5-ös és 3.6. |1.1.2 |

    A HDInsight-verziókról és összetevők további információkért lásd: [hello különböző Hadoop-összetevők és a HDInsight együttes rendelkezésre Mik](hdinsight-component-versioning.md).

3. Adja hozzá a következő kód toohello hello **pom.xml** fájlt. Ez a szöveg hello belül kell `<project>...</project>` hello címkék fájlba, például közötti `</dependencies>` és `</project>`.

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

    Ez a szakasz konfigurál egy erőforrást (`conf/hbase-site.xml`), amely a HBase konfigurációs információkat tartalmaz.

   > [!NOTE]
   > Megadhatja a konfigurációs értékeket keresztül kódot is. A hello hello megjegyzéseket `CreateTable` példa.

    Ez a szakasz is konfigurálja hello [Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/) és [Maven árnyalatát beépülő modul](http://maven.apache.org/plugins/maven-shade-plugin/). hello fordító beépülő modul használt toocompile hello topológia. hello árnyalatát beépülő modul használt tooprevent licenc ismétlődést hello JAR package Maven által épített. A beépülő modul futási időben hello HDInsight-fürt "ismétlődő licencfájlok" hiba használt tooprevent van. Maven-árnyalatát-beépülő modul használata hello `ApacheLicenseResourceTransformer` megvalósítási megakadályozza, hogy a hello hiba.

    maven-árnyalatát-beépülő modul hello is egy uber jar hello alkalmazás által igényelt összes hello függőségeket tartalmazó hoz létre.

4. Mentse a hello `pom.xml` fájlt.

5. Hozzon létre egy könyvtárat nevű `conf` a hello `hbaseapp` könyvtár. Ebben a könyvtárban használt toohold tooHBase kapcsolódás konfigurációs adatait.

6. Használjon hello következő parancsot a toocopy hello HBase-konfiguráció hello HBase fürt toohello `conf` könyvtár. Cserélje le `USERNAME` hello nevet, az SSH-bejelentkezéskor. Cserélje le `CLUSTERNAME` a HDInsight fürt nevű:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   További információk az `ssh` és `scp`, lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása

1. Nyissa meg toohello `hbaseapp/src/main/java/com/microsoft/examples` könyvtár, és nevezze át hello app.java fájl túl`CreateTable.java`.

2. Nyissa meg hello `CreateTable.java` fájlt, és cserélje le a meglévő tartalom hello hello szöveg a következő:

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

    Ez a kód hello **CreateTable** osztályt, amely egy nevű táblát hoz létre **személyek** , és feltöltheti olyan előre definiált felhasználók.

3. Mentse a hello `CreateTable.java` fájlt.

4. A hello `hbaseapp/src/main/java/com/microsoft/examples` könyvtár, hozzon létre egy fájlt `SearchByEmail.java`. Szöveg hello a fájl tartalmát, a következő hello használata:

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

    Hello **SearchByEmail** osztály használt tooquery sorok e-mail cím alapján lehet. Mivel a program reguláris kifejezés szűrőt, megadhatja egy karakterlánc vagy egy reguláris kifejezést hello osztály használata esetén.

5. Mentse a hello `SearchByEmail.java` fájlt.

6. A hello `hbaseapp/src/main/hava/com/microsoft/examples` könyvtár, hozzon létre egy fájlt `DeleteTable.java`. Szöveg hello a fájl tartalmát, a következő hello használata:

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

    Ez az osztály a szükségtelenné vált hello letiltásával ebben a példában létrehozott HBase táblákat és hello tábla eldobása hozta létre hello `CreateTable` osztály.

7. Mentse a hello `DeleteTable.java` fájlt.

## <a name="build-and-package-hello-application"></a>Buildelés és a csomag hello alkalmazás

1. A hello `hbaseapp` könyvtárába, használjon hello következő parancsot a toobuild hello alkalmazást tartalmazó JAR-fájlt:

    ```bash
    mvn clean package
    ```

    Ez a parancs létrehozza, és csomagok hello .jar fájl alkalmazást.

2. Hello parancs befejeződésekor hello `hbaseapp/target` directory nevű fájlt tartalmaz `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > Hello `hbaseapp-1.0-SNAPSHOT.jar` fájl egy uber jar. Minden hello függőségek szükséges toorun hello alkalmazást tartalmaz.


## <a name="upload-hello-jar-and-run-jobs-ssh"></a>Töltse fel a hello JAR-feladatok és futtatása (SSH)

a következő lépéseket használata hello `scp` toocopy hello JAR toohello elsődleges átjárócsomópontjához a HBase a HDInsight-fürthöz. Hello `ssh` parancs nem tooconnect toohello fürt használt, és futtassa közvetlenül hello példa hello átjárócsomópont.

1. tooupload hello jar toohello fürt, a következő parancs használata hello:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Cserélje le `USERNAME` hello nevet, az SSH-bejelentkezéskor. Cserélje le `CLUSTERNAME` elemet a HDInsight fürt nevére.

2. tooconnect toohello HBase-fürtöt, a következő parancs hello használata:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Cserélje le `USERNAME` az SSH-bejelentkezéskor hello nevét. Cserélje le `CLUSTERNAME` elemet a HDInsight fürt nevére.

3. egy HBase tábla használatával toocreate hello Java-alkalmazások, a következő parancs használata hello:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Ezzel a paranccsal létrejön egy HBase tábla nevű **személyek**, és tölti fel az adatokat.

4. az e-mail címeket a következő parancs használata hello hello táblában tárolt toosearch:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    A következő eredmények hello jelenhet meg:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. toodelete hello tábla, a következő parancs használata hello:

    

## <a name="upload-hello-jar-and-run-jobs-powershell"></a>Töltse fel a hello JAR-feladatok és futtatása (PowerShell)

a lépéseket követve hello Azure PowerShell tooupload hello JAR toohello alapértelmezett tárhelyet használja a HBase-fürtöt. HDInsight-parancsmagokat nem, akkor a használt toorun hello példák távolról.

1. Miután telepítése és konfigurálása az Azure PowerShell, hozzon létre egy fájlt `hbase-runner.psm1`. Szöveg hello a fájl tartalmát, a következő hello használata:

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

    Ez a fájl két lehetővé tevő modulokat tartalmaz:

   * **Adja hozzá HDInsightFile** - használt tooupload fájlok toohello fürt
   * **Start-HBaseExample** -használt korábban létrehozott toorun hello osztályok

2. Mentse a hello `hbase-runner.psm1` fájlt.

3. Nyisson meg egy új Azure PowerShell-ablakot, módosítsa a könyvtárakat toohello `hbaseapp` könyvtárat, majd futtatási hello végrehajtja a parancsot:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Módosítani hello elérési toohello hello `hbase-runner.psm1` korábban létrehozott fájlt. Ez a parancs az Azure PowerShell hello modul regisztrálása.

4. Használjon hello következő parancsot a tooupload hello `hbaseapp-1.0-SNAPSHOT.jar` tooyour fürt.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    Cserélje le `hdinsightclustername` hello néven a fürt. hello parancs feltölt hello `hbaseapp-1.0-SNAPSHOT.jar` toohello `example/jars` hello elsődleges a fürt tárolóhelyét helyét.

5. egy tábla használatával toocreate hello `hbaseapp`, a következő parancs hello használata:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    Cserélje le `hdinsightclustername` hello néven a fürt.

    Ezzel a paranccsal létrejön nevű tábla **személyek** a HBase a HDInsight-fürtre. Ez a parancs nem jeleníti meg a hello konzolablak kimenetet.

6. a hello táblázatban, a következő parancs használata hello toosearch:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    Cserélje le `hdinsightclustername` hello néven a fürt.

    Ez a parancs hello `SearchByEmail` toosearch egyetlen sort sem az osztály adott hello `contactinformation` oszlop termékcsalád és hello `email` oszlopban hello karakterláncot tartalmaz `contoso.com`. A következő eredmények hello kell megjelennie:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Használatával **fabrikam.com** a hello `-emailRegex` értéket adja vissza hello felhasználóknak, akik számára **fabrikam.com** hello e-mail mezőben. A reguláris kifejezések hello keresési kifejezés is használhatja. Például **^ r** értéket ad vissza e-mail címek hello "r" betűvel kezdődik.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Nincs eredményeit, illetve a Start-HBaseExample használata váratlan eredményekhez

Használjon hello `-showErr` paraméter tooview hello standard hiba (STDERR) futó hello feladat során előállított.

## <a name="delete-hello-table"></a>Hello tábla törlése

Amikor elkészült, a hello példa, használja a következő toodelete hello hello **személyek** ebben a példában használt tábla:

__Az egy `ssh` munkamenet__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Az Azure PowerShell__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Következő lépések

[Megtudhatja, hogyan toouse SQuirreL SQL, a hbase eszközzel](hdinsight-hbase-phoenix-squirrel-linux.md)
