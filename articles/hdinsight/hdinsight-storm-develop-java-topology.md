---
title: "aaaApache Storm például Java-topológiák - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate Apache Storm-topológiák Java nyelven hozzon létre egy példa word száma topológia."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "Apache storm, apache storm például storm java, a storm topológia – példa"
ms.assetid: a8838f29-9c08-4fd9-99ef-26655d1bf6d7
ms.service: hdinsight
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/07/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: 54fa9dc3c93ddad83ac861f3101f50f80117d804
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a><span data-ttu-id="7e208-104">Hozzon létre egy Apache Storm-topológia a Java nyelven</span><span class="sxs-lookup"><span data-stu-id="7e208-104">Create an Apache Storm topology in Java</span></span>

<span data-ttu-id="7e208-105">Megtudhatja, hogyan toocreate alatt futó Apache Storm egy Java-alapú topológiáját.</span><span class="sxs-lookup"><span data-stu-id="7e208-105">Learn how toocreate a Java-based topology for Apache Storm.</span></span> <span data-ttu-id="7e208-106">A Storm-topológia, amely egy word-count alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="7e208-106">You create a Storm topology that implements a word-count application.</span></span> <span data-ttu-id="7e208-107">Maven toobuild és a csomag hello projekt használja.</span><span class="sxs-lookup"><span data-stu-id="7e208-107">You use Maven toobuild and package hello project.</span></span> <span data-ttu-id="7e208-108">Ezt követően megismerheti, hogyan hello fluxus keretrendszer toodefine hello topológia használatával.</span><span class="sxs-lookup"><span data-stu-id="7e208-108">Then, you learn how toodefine hello topology using hello Flux framework.</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-109">hello fluxus keretrendszer elérhető Storm 0.10.0-s vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="7e208-109">hello Flux framework is available in Storm 0.10.0 or later.</span></span> <span data-ttu-id="7e208-110">A Storm 0.10.0-s HDInsight 3.3 és 3.4 érhető el.</span><span class="sxs-lookup"><span data-stu-id="7e208-110">Storm 0.10.0 is available with HDInsight 3.3 and 3.4.</span></span>

<span data-ttu-id="7e208-111">Ebben a dokumentumban hello lépések végrehajtását követően hello topológia tooApache HDInsight alatt futó Storm telepítheti.</span><span class="sxs-lookup"><span data-stu-id="7e208-111">After completing hello steps in this document, you can deploy hello topology tooApache Storm on HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-112">A befejezett hello Storm-topológia példák létre ebben a dokumentumban verziója érhető el [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="7e208-112">A completed version of hello Storm topology examples created in this document is available at [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e208-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7e208-113">Prerequisites</span></span>

* [<span data-ttu-id="7e208-114">Java fejlesztői készlet (JDK) 7-es verzió</span><span class="sxs-lookup"><span data-stu-id="7e208-114">Java Developer Kit (JDK) version 7</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* <span data-ttu-id="7e208-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven rendszer project build Java-projektek esetében.</span><span class="sxs-lookup"><span data-stu-id="7e208-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="7e208-116">Egy szövegszerkesztőben, vagy IDE.</span><span class="sxs-lookup"><span data-stu-id="7e208-116">A text editor or IDE.</span></span>

## <a name="configure-environment-variables"></a><span data-ttu-id="7e208-117">Környezeti változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e208-117">Configure environment variables</span></span>

<span data-ttu-id="7e208-118">hello következő környezeti változó lehet beállítani a Java és hello JDK telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="7e208-118">hello following environment variables may be set when you install Java and hello JDK.</span></span> <span data-ttu-id="7e208-119">Azonban ellenőrizni kell, hogy léteznek, illetve hello a rendszer tartozó helyes értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="7e208-119">However, you should check that they exist and that they contain hello correct values for your system.</span></span>

* <span data-ttu-id="7e208-120">**JAVA_HOME** -hello Java-futtatókörnyezet (JRE) futtató toohello directory kell mutatnia.</span><span class="sxs-lookup"><span data-stu-id="7e208-120">**JAVA_HOME** - should point toohello directory where hello Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="7e208-121">Például egy Unix vagy Linux terjesztési nem rendelkezhet hasonló érték túl`/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="7e208-121">For example, in a Unix or Linux distribution, it should have a value similar too`/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="7e208-122">A Windows rendszerben kellene hasonló érték túl`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="7e208-122">In Windows, it would have a value similar too`c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="7e208-123">**Elérési út** -elérési utak a következő hello tartalmaznia kell:</span><span class="sxs-lookup"><span data-stu-id="7e208-123">**PATH** - should contain hello following paths:</span></span>

  * <span data-ttu-id="7e208-124">**JAVA_HOME** (vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="7e208-124">**JAVA_HOME** (or hello equivalent path)</span></span>

  * <span data-ttu-id="7e208-125">**JAVA_HOME\bin** (vagy ezzel egyenértékű elérési hello)</span><span class="sxs-lookup"><span data-stu-id="7e208-125">**JAVA_HOME\bin** (or hello equivalent path)</span></span>

  * <span data-ttu-id="7e208-126">hello mappát, ahová a Maven telepítve van</span><span class="sxs-lookup"><span data-stu-id="7e208-126">hello directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="7e208-127">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e208-127">Create a Maven project</span></span>

<span data-ttu-id="7e208-128">Hello parancssorból használható hello következő parancsot a toocreate nevű Maven-projektté **WordCount**:</span><span class="sxs-lookup"><span data-stu-id="7e208-128">From hello command line, use hello following command toocreate a Maven project named **WordCount**:</span></span>

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> <span data-ttu-id="7e208-129">Ha a PowerShell használata esetén meg kell helyezze a`-D` az idézőjelek közé foglalt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="7e208-129">If you are using PowerShell, you must surround the`-D` parameters with double quotes.</span></span>
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

<span data-ttu-id="7e208-130">Ezzel a paranccsal létrejön egy nevű könyvtár `WordCount` hello aktuális helyen, amely tartalmazza egy alapszintű Maven project.</span><span class="sxs-lookup"><span data-stu-id="7e208-130">This command creates a directory named `WordCount` at hello current location, which contains a basic Maven project.</span></span> <span data-ttu-id="7e208-131">Hello `WordCount` könyvtár hello a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="7e208-131">hello `WordCount` directory contains hello following items:</span></span>

* <span data-ttu-id="7e208-132">`pom.xml`: Hello Maven project beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-132">`pom.xml`: Contains settings for hello Maven project.</span></span>
* <span data-ttu-id="7e208-133">`src\main\java\com\microsoft\example`: Az alkalmazás kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-133">`src\main\java\com\microsoft\example`: Contains your application code.</span></span>
* <span data-ttu-id="7e208-134">`src\test\java\com\microsoft\example`: Az alkalmazás a tesztek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-134">`src\test\java\com\microsoft\example`: Contains tests for your application.</span></span> 

### <a name="remove-hello-generated-example-code"></a><span data-ttu-id="7e208-135">Távolítsa el a generált hello példakód</span><span class="sxs-lookup"><span data-stu-id="7e208-135">Remove hello generated example code</span></span>

<span data-ttu-id="7e208-136">Generált hello tesztelése és hello alkalmazásfájlok törlése:</span><span class="sxs-lookup"><span data-stu-id="7e208-136">Delete hello generated test and hello application files:</span></span>

* <span data-ttu-id="7e208-137">**src\test\java\com\microsoft\example\AppTest.Java**</span><span class="sxs-lookup"><span data-stu-id="7e208-137">**src\test\java\com\microsoft\example\AppTest.java**</span></span>
* <span data-ttu-id="7e208-138">**src\main\java\com\microsoft\example\App.Java**</span><span class="sxs-lookup"><span data-stu-id="7e208-138">**src\main\java\com\microsoft\example\App.java**</span></span>

## <a name="add-maven-repositories"></a><span data-ttu-id="7e208-139">Adja hozzá a Maven tárházak</span><span class="sxs-lookup"><span data-stu-id="7e208-139">Add Maven repositories</span></span>

<span data-ttu-id="7e208-140">A HDInsight alatt futó Apache Storm projektjeikbe hello Hortonworks tárház toodownload függőségek használatát javasoljuk, hello Hortonworks Data Platform (HDP) alapul.</span><span class="sxs-lookup"><span data-stu-id="7e208-140">HDInsight is based on hello Hortonworks Data Platform (HDP), so we recommend using hello Hortonworks repository toodownload dependencies for your Apache Storm projects.</span></span> <span data-ttu-id="7e208-141">A hello __pom.xml__ fájlt, adja hozzá az XML hello után a következő hello `<url>http://maven.apache.org</url>` sor:</span><span class="sxs-lookup"><span data-stu-id="7e208-141">In hello __pom.xml__ file, add hello following XML after hello `<url>http://maven.apache.org</url>` line:</span></span>

```xml
<repositories>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPReleases</id>
        <name>HDP Releases</name>
        <url>http://repo.hortonworks.com/content/repositories/releases/</url>
        <layout>default</layout>
    </repository>
    <repository>
        <releases>
            <enabled>true</enabled>
            <updatePolicy>always</updatePolicy>
            <checksumPolicy>warn</checksumPolicy>
        </releases>
        <snapshots>
            <enabled>false</enabled>
            <updatePolicy>never</updatePolicy>
            <checksumPolicy>fail</checksumPolicy>
        </snapshots>
        <id>HDPJetty</id>
        <name>Hadoop Jetty</name>
        <url>http://repo.hortonworks.com/content/repositories/jetty-hadoop/</url>
        <layout>default</layout>
    </repository>
</repositories>
```

## <a name="add-properties"></a><span data-ttu-id="7e208-142">Tulajdonságok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7e208-142">Add properties</span></span>

<span data-ttu-id="7e208-143">Maven lehetővé teszi tulajdonságok nevű toodefine projektszintű értékeket.</span><span class="sxs-lookup"><span data-stu-id="7e208-143">Maven allows you toodefine project-level values called properties.</span></span> <span data-ttu-id="7e208-144">A hello __pom.xml__, adja hozzá a következő szöveg után hello hello `</repositories>` sor:</span><span class="sxs-lookup"><span data-stu-id="7e208-144">In hello __pom.xml__, add hello following text after hello `</repositories>` line:</span></span>

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from hello Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

<span data-ttu-id="7e208-145">Ezután már használhatja ezt az értéket a többi szakasza pedig hello `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="7e208-145">You can now use this value in other sections of hello `pom.xml`.</span></span> <span data-ttu-id="7e208-146">Például Storm-összetevőket hello verziójának meghatározásakor használhatja `${storm.version}` rögzített kódolási érték helyett.</span><span class="sxs-lookup"><span data-stu-id="7e208-146">For example, when specifying hello version of Storm components, you can use `${storm.version}` instead of hard coding a value.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="7e208-147">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="7e208-147">Add dependencies</span></span>

<span data-ttu-id="7e208-148">Vegyen fel egy függőséget Storm-összetevőket.</span><span class="sxs-lookup"><span data-stu-id="7e208-148">Add a dependency for Storm components.</span></span> <span data-ttu-id="7e208-149">Nyissa meg hello `pom.xml` fájlt, és adja hozzá a következő kódot a hello hello `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="7e208-149">Open hello `pom.xml` file and add hello following code in hello `<dependencies>` section:</span></span>

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of hello jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

<span data-ttu-id="7e208-150">Fordítási időben Maven ezen információk toolook felhasználásra kerül `storm-core` hello Maven tárházban.</span><span class="sxs-lookup"><span data-stu-id="7e208-150">At compile time, Maven uses this information toolook up `storm-core` in hello Maven repository.</span></span> <span data-ttu-id="7e208-151">Először a jelek hello tárházban a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7e208-151">It first looks in hello repository on your local computer.</span></span> <span data-ttu-id="7e208-152">Ha hello fájlokat nem létezik, a Maven adattárból hello nyilvános Maven letölti azokat, és tárolja őket a hello helyi tárházban.</span><span class="sxs-lookup"><span data-stu-id="7e208-152">If hello files aren't there, Maven downloads them from hello public Maven repository and stores them in hello local repository.</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-153">Értesítés hello `<scope>provided</scope>` sor ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="7e208-153">Notice hello `<scope>provided</scope>` line in this section.</span></span> <span data-ttu-id="7e208-154">Ez a beállítás arról értesíti a Maven tooexclude **storm-core** bármely JAR fájlok jönnek létre, mert azt hello rendszer biztosítja.</span><span class="sxs-lookup"><span data-stu-id="7e208-154">This setting tells Maven tooexclude **storm-core** from any JAR files that are created, because it is provided by hello system.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="7e208-155">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7e208-155">Build configuration</span></span>

<span data-ttu-id="7e208-156">Maven beépülő modulok lehetővé teszik a hello projekt toocustomize hello build fázisból áll.</span><span class="sxs-lookup"><span data-stu-id="7e208-156">Maven plug-ins allow you toocustomize hello build stages of hello project.</span></span> <span data-ttu-id="7e208-157">Például hogyan hello projekt lefordított vagy hogyan toopackage JAR-fájlra be azt.</span><span class="sxs-lookup"><span data-stu-id="7e208-157">For example, how hello project is compiled or how toopackage it into a JAR file.</span></span> <span data-ttu-id="7e208-158">Nyissa meg hello `pom.xml` fájlt, és adja hozzá a következő kódot közvetlenül felett hello hello `</project>` sor.</span><span class="sxs-lookup"><span data-stu-id="7e208-158">Open hello `pom.xml` file and add hello following code directly above hello `</project>` line.</span></span>

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

<span data-ttu-id="7e208-159">Ebben a szakaszban használt tooadd beépülő modulok, erőforrások és egyéb build-konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="7e208-159">This section is used tooadd plug-ins, resources, and other build configuration options.</span></span> <span data-ttu-id="7e208-160">A hello teljes körű referenciáért **pom.xml** fájl című [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span><span class="sxs-lookup"><span data-stu-id="7e208-160">For a full reference of hello **pom.xml** file, see [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span></span>

### <a name="add-plug-ins"></a><span data-ttu-id="7e208-161">Beépülő modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7e208-161">Add plug-ins</span></span>

<span data-ttu-id="7e208-162">Apache Storm-topológiák Java megvalósított, hello [Exec Maven beépülő modul](http://www.mojohaus.org/exec-maven-plugin/) akkor hasznos, mivel a tooeasily hello topológia futtassa helyileg a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="7e208-162">For Apache Storm topologies implemented in Java, hello [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) is useful because it allows you tooeasily run hello topology locally in your development environment.</span></span> <span data-ttu-id="7e208-163">Adja hozzá a következő toohello hello `<plugins>` hello szakasza `pom.xml` tooinclude hello Exec Maven beépülő modul fájlt:</span><span class="sxs-lookup"><span data-stu-id="7e208-163">Add hello following toohello `<plugins>` section of hello `pom.xml` file tooinclude hello Exec Maven plugin:</span></span>

```xml
<plugin>
    <groupId>org.codehaus.mojo</groupId>
    <artifactId>exec-maven-plugin</artifactId>
    <version>1.4.0</version>
    <executions>
    <execution>
    <goals>
        <goal>exec</goal>
    </goals>
    </execution>
    </executions>
    <configuration>
    <executable>java</executable>
    <includeProjectDependencies>true</includeProjectDependencies>
    <includePluginDependencies>false</includePluginDependencies>
    <classpathScope>compile</classpathScope>
    <mainClass>${storm.topology}</mainClass>
    <cleanupDaemonThreads>false</cleanupDaemonThreads> 
    </configuration>
</plugin>
```

<span data-ttu-id="7e208-164">Egy másik hasznos beépülő modul az hello [Apache Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/), mely van használt toochange fordítási lehetőségeket.</span><span class="sxs-lookup"><span data-stu-id="7e208-164">Another useful plug-in is hello [Apache Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), which is used toochange compilation options.</span></span> <span data-ttu-id="7e208-165">hello módosítások hello Maven által hello forrása és célja az alkalmazás a Java-verziót.</span><span class="sxs-lookup"><span data-stu-id="7e208-165">hello changes hello Java version that Maven uses for hello source and target for your application.</span></span>

* <span data-ttu-id="7e208-166">A HDInsight __3.4 vagy korábbi__, hello forrás beállítása és a Java-verzió too__1.7__ céloz.</span><span class="sxs-lookup"><span data-stu-id="7e208-166">For HDInsight __3.4 or earlier__, set hello source and target Java version too__1.7__.</span></span>

* <span data-ttu-id="7e208-167">A HDInsight __3.5__, hello forrás beállítása és a Java-verzió too__1.8__ céloz.</span><span class="sxs-lookup"><span data-stu-id="7e208-167">For HDInsight __3.5__, set hello source and target Java version too__1.8__.</span></span>

<span data-ttu-id="7e208-168">Adja hozzá a következő szöveget: hello hello `<plugins>` hello szakasza `pom.xml` tooinclude hello Apache Maven fordító beépülő modul fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e208-168">Add hello following text in hello `<plugins>` section of hello `pom.xml` file tooinclude hello Apache Maven Compiler plugin.</span></span> <span data-ttu-id="7e208-169">Ez a példa 1.8, határozza meg, így hello HDInsight célverzió 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="7e208-169">This example specifies 1.8, so hello target HDInsight version is 3.5.</span></span>

```xml
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.3</version>
    <configuration>
    <source>1.8</source>
    <target>1.8</target>
    </configuration>
</plugin>
```

### <a name="configure-resources"></a><span data-ttu-id="7e208-170">Erőforrások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e208-170">Configure resources</span></span>

<span data-ttu-id="7e208-171">hello erőforrások szakasz lehetővé teszi tooinclude nem kód erőforrások például összetevők hello topológia számára szükséges konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="7e208-171">hello resources section allows you tooinclude non-code resources such as configuration files needed by components in hello topology.</span></span> <span data-ttu-id="7e208-172">Ennél a példánál adja hozzá a következő szöveget: hello hello `<resources>` hello szakasza "pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e208-172">For this example, add hello following text in hello `<resources>` section of hello \`pom.xml file.</span></span>

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

<span data-ttu-id="7e208-173">Ez a példa hello erőforrások könyvtárat ad hello hello projekt gyökerében található (`${basedir}`) olyan erőforrásokat tartalmaz, és a hello fájlt tartalmazó helyként `log4j2.xml`.</span><span class="sxs-lookup"><span data-stu-id="7e208-173">This example adds hello resources directory in hello root of hello project (`${basedir}`) as a location that contains resources, and includes hello file named `log4j2.xml`.</span></span> <span data-ttu-id="7e208-174">A fájl használt tooconfigure, milyen információt naplózta hello topológia.</span><span class="sxs-lookup"><span data-stu-id="7e208-174">This file is used tooconfigure what information is logged by hello topology.</span></span>

## <a name="create-hello-topology"></a><span data-ttu-id="7e208-175">Hozzon létre hello topológia</span><span class="sxs-lookup"><span data-stu-id="7e208-175">Create hello topology</span></span>

<span data-ttu-id="7e208-176">A Java-alapú Apache Storm-topológia áll meg kell írni három összetevő (vagy hivatkozás) a függőség beállításához.</span><span class="sxs-lookup"><span data-stu-id="7e208-176">A Java-based Apache Storm topology consists of three components that you must author (or reference) as a dependency.</span></span>

* <span data-ttu-id="7e208-177">**Spoutok**: olvassa be a külső adatokat adatforrásokat, és megfelelően kibocsát adatstreamek hello topológia be.</span><span class="sxs-lookup"><span data-stu-id="7e208-177">**Spouts**: Reads data from external sources and emits streams of data into hello topology.</span></span>

* <span data-ttu-id="7e208-178">**Boltok**: feldolgozási végez spoutokkal kapcsolatban, vagy más boltokhoz által kibocsátott adatfolyamokat, és megfelelően kibocsát egy vagy több adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="7e208-178">**Bolts**: Performs processing on streams emitted by spouts or other bolts, and emits one or more streams.</span></span>

* <span data-ttu-id="7e208-179">**Topológia**: hogyan hello spoutok boltokhoz vannak rendezve, és és hello belépési pontot nyújt hello topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="7e208-179">**Topology**: Defines how hello spouts and bolts are arranged, and provides hello entry point for hello topology.</span></span>

### <a name="create-hello-spout"></a><span data-ttu-id="7e208-180">Hello spout létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e208-180">Create hello spout</span></span>

<span data-ttu-id="7e208-181">külső adatforrások, tooreduce követelményei hello következő spout egyszerűen véletlenszerű mondat bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="7e208-181">tooreduce requirements for setting up external data sources, hello following spout simply emits random sentences.</span></span> <span data-ttu-id="7e208-182">Egy spout hello mellékelt módosított változatát [Storm-kezdőpéldák](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span><span class="sxs-lookup"><span data-stu-id="7e208-182">It is a modified version of a spout that is provided with hello [Storm-Starter examples](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-183">Például egy olyan spout egy külső adatforrásból olvasó tekintse meg a következő példák hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="7e208-183">For an example of a spout that reads from an external data source, see one of hello following examples:</span></span>
>
> * <span data-ttu-id="7e208-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): egy példa spout, amely Twitter olvassa be</span><span class="sxs-lookup"><span data-stu-id="7e208-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): An example spout that reads from Twitter</span></span>
> * <span data-ttu-id="7e208-185">[A Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): egy spout, amely Kafka olvassa be</span><span class="sxs-lookup"><span data-stu-id="7e208-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): A spout that reads from Kafka</span></span>

<span data-ttu-id="7e208-186">A hello spout, hozzon létre egy fájlt `RandomSentenceSpout.java` a hello `src\main\java\com\microsoft\example` Java-kóddal hello tartalmát, a következő könyvtárra, és használja hello:</span><span class="sxs-lookup"><span data-stu-id="7e208-186">For hello spout, create a file named `RandomSentenceSpout.java` in hello `src\main\java\com\microsoft\example` directory and use hello following Java code as hello contents:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.spout.SpoutOutputCollector;
import org.apache.storm.task.TopologyContext;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseRichSpout;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Values;
import org.apache.storm.utils.Utils;

import java.util.Map;
import java.util.Random;

//This spout randomly emits sentences
public class RandomSentenceSpout extends BaseRichSpout {
  //Collector used tooemit output
  SpoutOutputCollector _collector;
  //Used toogenerate a random number
  Random _rand;

  //Open is called when an instance of hello class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set hello instance collector toohello one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data toohello stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //hello sentences that are randomly emitted
    String[] sentences = new String[]{ "hello cow jumped over hello moon", "an apple a day keeps hello doctor away",
        "four score and seven years ago", "snow white and hello seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit hello sentence
    _collector.emit(new Values(sentence));
  }

  //Ack is not implemented since this is a basic example
  @Override
  public void ack(Object id) {
  }

  //Fail is not implemented since this is a basic example
  @Override
  public void fail(Object id) {
  }

  //Declare hello output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> <span data-ttu-id="7e208-187">Bár ez a topológia csak egy spout, mások is rendelkezhet, amely adatokat különböző forrásokból történő hello topológia több.</span><span class="sxs-lookup"><span data-stu-id="7e208-187">Although this topology uses only one spout, others may have several that feed data from different sources into hello topology.</span></span>

### <a name="create-hello-bolts"></a><span data-ttu-id="7e208-188">Hello boltokhoz létrehozása</span><span class="sxs-lookup"><span data-stu-id="7e208-188">Create hello bolts</span></span>

<span data-ttu-id="7e208-189">Szögek hello adatfeldolgozási kezelni.</span><span class="sxs-lookup"><span data-stu-id="7e208-189">Bolts handle hello data processing.</span></span> <span data-ttu-id="7e208-190">Ez a topológia két boltokhoz használja:</span><span class="sxs-lookup"><span data-stu-id="7e208-190">This topology uses two bolts:</span></span>

* <span data-ttu-id="7e208-191">**SplitSentence**: hello mondat által kibocsátott felosztja **RandomSentenceSpout** az egyes szavakat.</span><span class="sxs-lookup"><span data-stu-id="7e208-191">**SplitSentence**: Splits hello sentences emitted by **RandomSentenceSpout** into individual words.</span></span>

* <span data-ttu-id="7e208-192">**WordCount**: hányszor történt minden szó található.</span><span class="sxs-lookup"><span data-stu-id="7e208-192">**WordCount**: Counts how many times each word has occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-193">Szögek is végrehajthat, például számítási, adatmegőrzés vagy tooexternal összetevők van szó.</span><span class="sxs-lookup"><span data-stu-id="7e208-193">Bolts can do anything, for example, computation, persistence, or talking tooexternal components.</span></span>

<span data-ttu-id="7e208-194">Hozzon létre két új fájl `SplitSentence.java` és `WordCount.java` a hello `src\main\java\com\microsoft\example` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7e208-194">Create two new files, `SplitSentence.java` and `WordCount.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="7e208-195">Szöveg hello fájlok hello tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="7e208-195">Use hello following text as hello contents for hello files:</span></span>

#### <a name="splitsentence"></a><span data-ttu-id="7e208-196">SplitSentence</span><span class="sxs-lookup"><span data-stu-id="7e208-196">SplitSentence</span></span>

```java
package com.microsoft.example;

import java.text.BreakIterator;

import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class SplitSentence extends BaseBasicBolt {

  //Execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get hello sentence content from hello tuple
    String sentence = tuple.getString(0);
    //An iterator tooget each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give hello iterator hello sentence
    boundary.setText(sentence);
    //Find hello beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it toohello output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get hello word
      String word=sentence.substring(start,end);
      //If a word is whitespace characters, replace it with empty
      word=word.replaceAll("\\s+","");
      //if it's an actual word, emit it
      if (!word.equals("")) {
        collector.emit(new Values(word));
      }
    }
  }

  //Declare that emitted tuples contain a word field
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word"));
  }
}
```

#### <a name="wordcount"></a><span data-ttu-id="7e208-197">WordCount</span><span class="sxs-lookup"><span data-stu-id="7e208-197">WordCount</span></span>

```java
package com.microsoft.example;

import java.util.HashMap;
import java.util.Map;
import java.util.Iterator;

import org.apache.storm.Constants;
import org.apache.storm.topology.BasicOutputCollector;
import org.apache.storm.topology.OutputFieldsDeclarer;
import org.apache.storm.topology.base.BaseBasicBolt;
import org.apache.storm.tuple.Fields;
import org.apache.storm.tuple.Tuple;
import org.apache.storm.tuple.Values;
import org.apache.storm.Config;

// For logging
import org.apache.logging.log4j.Logger;
import org.apache.logging.log4j.LogManager;

//There are a variety of bolt types. In this case, use BaseBasicBolt
public class WordCount extends BaseBasicBolt {
  //Create logger for this class
  private static final Logger logger = LogManager.getLogger(WordCount.class);
  //For holding words and counts
  Map<String, Integer> counts = new HashMap<String, Integer>();
  //How often tooemit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default too60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used tootrigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called tooprocess tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //If it's a tick tuple, emit all words and counts
    if(tuple.getSourceComponent().equals(Constants.SYSTEM_COMPONENT_ID)
            && tuple.getSourceStreamId().equals(Constants.SYSTEM_TICK_STREAM_ID)) {
      for(String word : counts.keySet()) {
        Integer count = counts.get(word);
        collector.emit(new Values(word, count));
        logger.info("Emitting a count of " + count + " for word " + word);
      }
    } else {
      //Get hello word contents from hello tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment hello count and store it
      count++;
      counts.put(word, count);
    }
  }

  //Declare that this emits a tuple containing two fields; word and count
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("word", "count"));
  }
}
```

### <a name="define-hello-topology"></a><span data-ttu-id="7e208-198">Hello topológia meghatározása</span><span class="sxs-lookup"><span data-stu-id="7e208-198">Define hello topology</span></span>

<span data-ttu-id="7e208-199">hello topológia kötelékek hello spoutokkal kapcsolatban, és a grafikon, amely meghatározza, hogyan közötti adatáramlás hello összetevők együttesen boltok.</span><span class="sxs-lookup"><span data-stu-id="7e208-199">hello topology ties hello spouts and bolts together into a graph, which defines how data flows between hello components.</span></span> <span data-ttu-id="7e208-200">Párhuzamossági mutatók Storm használó hello fürtön belül hello összetevők példányai létrehozásakor is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-200">It also provides parallelism hints that Storm uses when creating instances of hello components within hello cluster.</span></span>

<span data-ttu-id="7e208-201">hello példánycsoportokat egy egyszerű diagram hello gráf ebben a topológiában az összetevőt.</span><span class="sxs-lookup"><span data-stu-id="7e208-201">hello following image is a basic diagram of hello graph of components for this topology.</span></span>

![diagram ábrázoló hello spoutok és boltok elrendezése](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

<span data-ttu-id="7e208-203">tooimplement hello topológia, hozzon létre egy fájlt `WordCountTopology.java` a hello `src\main\java\com\microsoft\example` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7e208-203">tooimplement hello topology, create a file named `WordCountTopology.java` in hello `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="7e208-204">Java-kóddal hello hello fájl tartalmát, a következő hello használata:</span><span class="sxs-lookup"><span data-stu-id="7e208-204">Use hello following Java code as hello contents of hello file:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for hello topology
  public static void main(String[] args) throws Exception {
  //Used toobuild hello topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add hello spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add hello SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes toohello spout, and equally distributes
    //tuples (sentences) across instances of hello SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add hello counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes toohello split bolt, and
    //ensures that hello same word is sent toohello same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set toofalse toodisable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint tooset hello number of workers
      conf.setNumWorkers(3);
      //submit hello topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap hello maximum number of executors that can be spawned
      //for a component too3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used toorun locally
      LocalCluster cluster = new LocalCluster();
      //submit hello topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down hello cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a><span data-ttu-id="7e208-205">Naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7e208-205">Configure logging</span></span>

<span data-ttu-id="7e208-206">A Storm Apache Log4j toolog információkat használja.</span><span class="sxs-lookup"><span data-stu-id="7e208-206">Storm uses Apache Log4j toolog information.</span></span> <span data-ttu-id="7e208-207">Ha nem konfigurálja a naplózási, hello topológia diagnosztikai adatokat bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="7e208-207">If you do not configure logging, hello topology emits diagnostic information.</span></span> <span data-ttu-id="7e208-208">Mi kerül toocontrol hozzon létre egy fájlt `log4j2.xml` a hello `resources` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="7e208-208">toocontrol what is logged, create a file named `log4j2.xml` in hello `resources` directory.</span></span> <span data-ttu-id="7e208-209">XML hello hello fájl tartalmát, a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="7e208-209">Use hello following XML as hello contents of hello file.</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration>
<Appenders>
    <Console name="STDOUT" target="SYSTEM_OUT">
        <PatternLayout pattern="%d{HH:mm:ss} [%t] %-5level %logger{36} - %msg%n"/>
    </Console>
</Appenders>
<Loggers>
    <Logger name="com.microsoft.example" level="trace" additivity="false">
        <AppenderRef ref="STDOUT"/>
    </Logger>
    <Root level="error">
        <Appender-Ref ref="STDOUT"/>
    </Root>
</Loggers>
</Configuration>
```

<span data-ttu-id="7e208-210">Az XML-kód konfigurálja egy új naplózó a hello `com.microsoft.example` osztályt, amely ebben a példában topológiában hello összetevőket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-210">This XML configures a new logger for hello `com.microsoft.example` class, which includes hello components in this example topology.</span></span> <span data-ttu-id="7e208-211">hello szintje tootrace a a tranzakciónaplókat tartalmazó, amely minden ebben a topológiában-összetevők által kibocsátott naplózási információkat rögzíti.</span><span class="sxs-lookup"><span data-stu-id="7e208-211">hello level is set tootrace for this logger, which captures any logging information emitted by components in this topology.</span></span>

<span data-ttu-id="7e208-212">Hello `<Root level="error">` szakasz hello legfelső szintű a naplózási szint konfigurálása (nem a minden `com.microsoft.example`) tooonly naplózási hiba adatok.</span><span class="sxs-lookup"><span data-stu-id="7e208-212">hello `<Root level="error">` section configures hello root level of logging (everything not in `com.microsoft.example`) tooonly log error information.</span></span>

<span data-ttu-id="7e208-213">Log4j naplózásának konfigurálásáról további információkért lásd: [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="7e208-213">For more information on configuring logging for Log4j, see [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span></span>

> [!NOTE]
> <span data-ttu-id="7e208-214">Storm verzióját 0.10.0-s és magasabb használata Log4j 2.x.</span><span class="sxs-lookup"><span data-stu-id="7e208-214">Storm version 0.10.0 and higher use Log4j 2.x.</span></span> <span data-ttu-id="7e208-215">A storm régebbi verzióit használja Log4j 1.x, a naplózási konfiguráció más formátumú használt.</span><span class="sxs-lookup"><span data-stu-id="7e208-215">Older versions of storm used Log4j 1.x, which used a different format for log configuration.</span></span> <span data-ttu-id="7e208-216">Hello régebbi konfigurációtól tudnivalókért lásd: [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span><span class="sxs-lookup"><span data-stu-id="7e208-216">For information on hello older configuration, see [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span></span>

## <a name="test-hello-topology-locally"></a><span data-ttu-id="7e208-217">Teszt hello helyileg topológia</span><span class="sxs-lookup"><span data-stu-id="7e208-217">Test hello topology locally</span></span>

<span data-ttu-id="7e208-218">Hello fájlok mentése után használja a következő parancs tootest hello topológia helyileg hello.</span><span class="sxs-lookup"><span data-stu-id="7e208-218">After you save hello files, use hello following command tootest hello topology locally.</span></span>

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

<span data-ttu-id="7e208-219">Futtatja, a hello topológia indítási információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7e208-219">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="7e208-220">hello következő szövege hello word-count kimeneti példát:</span><span class="sxs-lookup"><span data-stu-id="7e208-220">hello following text is an example of hello word count output:</span></span>

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

<span data-ttu-id="7e208-221">Ez a példa napló azt jelzi, hogy hello word "és" 113 alkalommal kibocsátását.</span><span class="sxs-lookup"><span data-stu-id="7e208-221">This example log indicates that hello word 'and' has been emitted 113 times.</span></span> <span data-ttu-id="7e208-222">hello száma továbbra is toogo be, amíg hello topológia fut, mert hello spout folyamatosan hello bocsát ki ugyanazt a mondatok.</span><span class="sxs-lookup"><span data-stu-id="7e208-222">hello count continues toogo up as long as hello topology runs because hello spout continuously emits hello same sentences.</span></span>

<span data-ttu-id="7e208-223">Egy 5 másodperces időköze van szó kibocsátási és a számok között.</span><span class="sxs-lookup"><span data-stu-id="7e208-223">There is a 5-second interval between emission of words and counts.</span></span> <span data-ttu-id="7e208-224">Hello **WordCount** -összetevő tooonly adatok létrehozása, ha egy rekordot érkezik.</span><span class="sxs-lookup"><span data-stu-id="7e208-224">hello **WordCount** component is configured tooonly emit information when a tick tuple arrives.</span></span> <span data-ttu-id="7e208-225">Adott listának csak kézbesítési öt másodpercenként osztásjelek kéri.</span><span class="sxs-lookup"><span data-stu-id="7e208-225">It requests that tick tuples are only delivered every five seconds.</span></span>

## <a name="convert-hello-topology-tooflux"></a><span data-ttu-id="7e208-226">Hello topológia tooFlux átalakítása</span><span class="sxs-lookup"><span data-stu-id="7e208-226">Convert hello topology tooFlux</span></span>

<span data-ttu-id="7e208-227">Fluxus egy új keretében elérhető Storm 0.10.0-s vagy újabb, amely lehetővé teszi a megvalósítás tooseparate konfigurációja a rendszer.</span><span class="sxs-lookup"><span data-stu-id="7e208-227">Flux is a new framework available with Storm 0.10.0 and higher, which allows you tooseparate configuration from implementation.</span></span> <span data-ttu-id="7e208-228">Az összetevők továbbra is Java vannak definiálva, de hello topológia definíciója YAM-fájllal.</span><span class="sxs-lookup"><span data-stu-id="7e208-228">Your components are still defined in Java, but hello topology is defined using a YAML file.</span></span> <span data-ttu-id="7e208-229">Csomagdefiníció egy alapértelmezett topológia a projektet, vagy egy önálló fájlt használja, hello topológia elküldésekor.</span><span class="sxs-lookup"><span data-stu-id="7e208-229">You can package a default topology definition with your project, or use a standalone file when submitting hello topology.</span></span> <span data-ttu-id="7e208-230">Hello topológia tooStorm elküldésekor hello YAM topológia meghatározása környezeti változókat vagy konfigurációs fájlok toopopulate értékek használhatja.</span><span class="sxs-lookup"><span data-stu-id="7e208-230">When submitting hello topology tooStorm, you can use environment variables or configuration files toopopulate values in hello YAML topology definition.</span></span>

<span data-ttu-id="7e208-231">hello YAM fájl hello összetevők toouse hello topológia és a közöttük hello adatfolyama határozza meg.</span><span class="sxs-lookup"><span data-stu-id="7e208-231">hello YAML file defines hello components toouse for hello topology and hello data flow between them.</span></span> <span data-ttu-id="7e208-232">Megadhat egy YAM fájl hello jar-fájl részeként, vagy egy külső YAM-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="7e208-232">You can include a YAML file as part of hello jar file or you can use an external YAML file.</span></span>

<span data-ttu-id="7e208-233">Fluxus további információkért lásd: [fluxus keretrendszer (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="7e208-233">For more information on Flux, see [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

> [!WARNING]
> <span data-ttu-id="7e208-234">Esedékes tooa [hiba (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) Storm 1.0.1-es, szükség lehet a tooinstall egy [Storm fejlesztőkörnyezet](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun fluxus topológiák helyileg.</span><span class="sxs-lookup"><span data-stu-id="7e208-234">Due tooa [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) with Storm 1.0.1, you may need tooinstall a [Storm development environment](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) toorun Flux topologies locally.</span></span>

1. <span data-ttu-id="7e208-235">Helyezze át a hello `WordCountTopology.java` fájlt kívüli hello projekt.</span><span class="sxs-lookup"><span data-stu-id="7e208-235">Move hello `WordCountTopology.java` file out of hello project.</span></span> <span data-ttu-id="7e208-236">Korábban a fájl a hello topológia definiálva, de a fluxus nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e208-236">Previously, this file defined hello topology, but isn't needed with Flux.</span></span>

2. <span data-ttu-id="7e208-237">A hello `resources` könyvtár, hozzon létre egy fájlt `topology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="7e208-237">In hello `resources` directory, create a file named `topology.yaml`.</span></span> <span data-ttu-id="7e208-238">Szöveg hello a fájl tartalmát, a következő hello használata.</span><span class="sxs-lookup"><span data-stu-id="7e208-238">Use hello following text as hello contents of this file.</span></span>

        name: "wordcount"       # friendly name for hello topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for hello number of workers toocreate
        
        spouts:                 # Spout definitions
        - id: "sentence-spout"
            className: "com.microsoft.example.RandomSentenceSpout"
            parallelism: 1      # parallelism hint
        
        bolts:                  # Bolt definitions
        - id: "splitter-bolt"
            className: "com.microsoft.example.SplitSentence"
            parallelism: 1
         
        - id: "counter-bolt"
            className: "com.microsoft.example.WordCount"
            constructorArgs:
                - 10
            parallelism: 1
        
        streams:                # Stream definitions
            - name: "Spout --> Splitter" # name isn't used (placeholder for logging, UI, etc.)
            from: "sentence-spout"       # hello stream emitter
            to: "splitter-bolt"          # hello stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) toogroup on

3. <span data-ttu-id="7e208-239">Ellenőrizze a következő módosításokat toohello hello `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e208-239">Make hello following changes toohello `pom.xml` file.</span></span>
   
   * <span data-ttu-id="7e208-240">Adja hozzá a következő új függőséghez hello hello `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="7e208-240">Add hello following new dependency in hello `<dependencies>` section:</span></span>
     
        ```xml
        <!-- Add a dependency on hello Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * <span data-ttu-id="7e208-241">Adja hozzá a következő beépülő modul toohello hello `<plugins>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="7e208-241">Add hello following plugin toohello `<plugins>` section.</span></span> <span data-ttu-id="7e208-242">A beépülő modul hello projekt hello létrehozása (jar-fájlt) csomag kezeli, és néhány átalakítások adott tooFlux vonatkozik hello csomag létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="7e208-242">This plugin handles hello creation of a package (jar file) for hello project, and applies some transformations specific tooFlux when creating hello package.</span></span>
     
        ```xml
        <!-- build an uber jar -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
                <transformers>
                    <!-- Keep us from getting a "can't overwrite file error" -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer" />
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ServicesResourceTransformer" />
                    <!-- We're using Flux, so refer tooit as main -->
                    <transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                        <mainClass>org.apache.storm.flux.Flux</mainClass>
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
        ```

   * <span data-ttu-id="7e208-243">A hello **exec-maven-beépülő modul** `<configuration>` területen hello értékének módosításához `<mainClass>` túl`org.apache.storm.flux.Flux`.</span><span class="sxs-lookup"><span data-stu-id="7e208-243">In hello **exec-maven-plugin** `<configuration>` section, change hello value for `<mainClass>` too`org.apache.storm.flux.Flux`.</span></span> <span data-ttu-id="7e208-244">Ez a beállítás lehetővé teszi, hogy a hello topológia helyben fut a fejlesztési fluxus toohandle.</span><span class="sxs-lookup"><span data-stu-id="7e208-244">This setting allows Flux toohandle running hello topology locally in development.</span></span>

   * <span data-ttu-id="7e208-245">A hello `<resources>` területen írja be a következő toohello hello `<includes>`.</span><span class="sxs-lookup"><span data-stu-id="7e208-245">In hello `<resources>` section, add hello following toohello `<includes>`.</span></span> <span data-ttu-id="7e208-246">Az XML-kód hello YAM definícióját tartalmazó hello topológia hello projekt részeként tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e208-246">This XML includes hello YAML file that defines hello topology as part of hello project.</span></span>

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-hello-flux-topology-locally"></a><span data-ttu-id="7e208-247">Hello fluxus topológia helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="7e208-247">Test hello flux topology locally</span></span>

1. <span data-ttu-id="7e208-248">A következő toocompile hello használja, és hajtsa végre a hello fluxus topológia Maven használatával:</span><span class="sxs-lookup"><span data-stu-id="7e208-248">Use hello following toocompile and execute hello Flux topology using Maven:</span></span>

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    <span data-ttu-id="7e208-249">PowerShell, a következő parancs használata hello használata:</span><span class="sxs-lookup"><span data-stu-id="7e208-249">If you are using PowerShell, use hello following command:</span></span>

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > <span data-ttu-id="7e208-250">Ha a topológia a Storm 1.0.1-es bits használ, ez a parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="7e208-250">If your topology uses Storm 1.0.1 bits, this command fails.</span></span> <span data-ttu-id="7e208-251">Ez a hiba oka [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span><span class="sxs-lookup"><span data-stu-id="7e208-251">This failure is caused by [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span></span> <span data-ttu-id="7e208-252">Ehelyett [Storm a fejlesztési környezet telepítése](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) és hello használja a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="7e208-252">Instead, [install Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) and use hello following information.</span></span>

    <span data-ttu-id="7e208-253">Ha rendelkezik [Storm a fejlesztési környezetben telepített](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), ehelyett a következő parancsok hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="7e208-253">If you have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use hello following commands instead:</span></span>

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    <span data-ttu-id="7e208-254">Hello `--local` paraméter hello topológia módban fut helyi a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="7e208-254">hello `--local` parameter runs hello topology in local mode on your development environment.</span></span> <span data-ttu-id="7e208-255">Hello `-R /topology.yaml` paramétert használ hello `topology.yaml` hello jar fájl toodefine hello topológia erőforrás fájlt.</span><span class="sxs-lookup"><span data-stu-id="7e208-255">hello `-R /topology.yaml` parameter uses hello `topology.yaml` file resource from hello jar file toodefine hello topology.</span></span>

    <span data-ttu-id="7e208-256">Futtatja, a hello topológia indítási információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7e208-256">As it runs, hello topology displays startup information.</span></span> <span data-ttu-id="7e208-257">a következő szöveg hello hello kimeneti példája:</span><span class="sxs-lookup"><span data-stu-id="7e208-257">hello following text is an example of hello output:</span></span>

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    <span data-ttu-id="7e208-258">Van egy 10 másodperces késleltetési kötegek naplózott adatok között.</span><span class="sxs-lookup"><span data-stu-id="7e208-258">There is a 10-second delay between batches of logged information.</span></span>

2. <span data-ttu-id="7e208-259">Másolatot készít hello `topology.yaml` fájl hello projektből.</span><span class="sxs-lookup"><span data-stu-id="7e208-259">Make a copy of hello `topology.yaml` file from hello project.</span></span> <span data-ttu-id="7e208-260">Nevű hello új fájl `newtopology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="7e208-260">Name hello new file `newtopology.yaml`.</span></span> <span data-ttu-id="7e208-261">A hello `newtopology.yaml` fájlt, keresse meg hello következő szakaszt, és hello értékének módosítása `10` túl`5`.</span><span class="sxs-lookup"><span data-stu-id="7e208-261">In hello `newtopology.yaml` file, find hello following section and change hello value of `10` too`5`.</span></span> <span data-ttu-id="7e208-262">A módosítás módosítások hello időközétől kibocsátó Word kötegek száma 10 másodperc too5 a.</span><span class="sxs-lookup"><span data-stu-id="7e208-262">This modification changes hello interval between emitting batches of word counts from 10 seconds too5.</span></span>

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. toorun hello topology, use hello following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    <span data-ttu-id="7e208-263">Vagy ha a fejlesztési környezet Storm rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="7e208-263">Or, if you have Storm on your development environment:</span></span>

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    <span data-ttu-id="7e208-264">Változás hello `/path/to/newtopology.yaml` toohello elérési út toohello newtopology.yaml fájl hello előző lépésben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="7e208-264">Change hello `/path/to/newtopology.yaml` toohello path toohello newtopology.yaml file you created in hello previous step.</span></span> <span data-ttu-id="7e208-265">Ez a parancs hello newtopology.yaml hello topológia definíciófrissítések használja.</span><span class="sxs-lookup"><span data-stu-id="7e208-265">This command uses hello newtopology.yaml as hello topology definition.</span></span> <span data-ttu-id="7e208-266">Mivel nem magában foglalja az hello `compile` paraméter, a Maven hello projektet egy előző lépésekben hello verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="7e208-266">Since we didn't include hello `compile` parameter, Maven uses hello version of hello project built in previous steps.</span></span>

    <span data-ttu-id="7e208-267">Egyszer hello topológia elindul, kell figyelje meg, hogy a hello idő közötti kibocsátott kötegek newtopology.yaml tooreflect hello értéke megváltozott.</span><span class="sxs-lookup"><span data-stu-id="7e208-267">Once hello topology starts, you should notice that hello time between emitted batches has changed tooreflect hello value in newtopology.yaml.</span></span> <span data-ttu-id="7e208-268">Így tudja módosítani a konfigurációs YAM fájl anélkül, hogy toorecompile hello topológia látható.</span><span class="sxs-lookup"><span data-stu-id="7e208-268">So you can see that you can change your configuration through a YAML file without having toorecompile hello topology.</span></span>

<span data-ttu-id="7e208-269">Ezeket és más szolgáltatások hello fluxus keretrendszer további információkért lásd: [fluxus (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="7e208-269">For more information on these and other features of hello Flux framework, see [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

## <a name="trident"></a><span data-ttu-id="7e208-270">Trident</span><span class="sxs-lookup"><span data-stu-id="7e208-270">Trident</span></span>

<span data-ttu-id="7e208-271">Trident egy magas szintű absztrakció, Storm által biztosított.</span><span class="sxs-lookup"><span data-stu-id="7e208-271">Trident is a high-level abstraction that is provided by Storm.</span></span> <span data-ttu-id="7e208-272">Állapot-nyilvántartó feldolgozási támogatja.</span><span class="sxs-lookup"><span data-stu-id="7e208-272">It supports stateful processing.</span></span> <span data-ttu-id="7e208-273">hello elsődleges Trident előnye, hogy azt is garantálni hello topológia kerül minden üzenetet csak egyszer dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="7e208-273">hello primary advantage of Trident is that it can guarantee that every message that enters hello topology is processed only once.</span></span> <span data-ttu-id="7e208-274">A Trident ezzel szemben nélkül a topológia is csak garantálja, hogy az üzenetek legalább egyszer fel.</span><span class="sxs-lookup"><span data-stu-id="7e208-274">Without using Trident, your topology can only guarantee that messages are processed at least once.</span></span> <span data-ttu-id="7e208-275">Is más különbségek vannak, például a beépített összetevők boltokhoz létrehozása helyett használható.</span><span class="sxs-lookup"><span data-stu-id="7e208-275">There are also other differences, such as built-in components that can be used instead of creating bolts.</span></span> <span data-ttu-id="7e208-276">Valójában boltokhoz kevésbé általános összetevők, például a szűrőket, a leképezések és a funkciók helyébe lép.</span><span class="sxs-lookup"><span data-stu-id="7e208-276">In fact, bolts are replaced by less-generic components, such as filters, projections, and functions.</span></span>

<span data-ttu-id="7e208-277">Trident alkalmazások Maven-projektek használatával is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="7e208-277">Trident applications can be created by using Maven projects.</span></span> <span data-ttu-id="7e208-278">Hello használata azonos basic az ebben a cikkben bemutatott lépések – csak hello kód nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="7e208-278">You use hello same basic steps as presented earlier in this article—only hello code is different.</span></span> <span data-ttu-id="7e208-279">Trident is nem (jelenleg) használható hello fluxus keretrendszer.</span><span class="sxs-lookup"><span data-stu-id="7e208-279">Trident also cannot (currently) be used with hello Flux framework.</span></span>

<span data-ttu-id="7e208-280">A Trident kapcsolatos további információkért lásd: hello [Trident API – áttekintés](http://storm.apache.org/documentation/Trident-API-Overview.html).</span><span class="sxs-lookup"><span data-stu-id="7e208-280">For more information about Trident, see hello [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html).</span></span>

<span data-ttu-id="7e208-281">A Trident alkalmazás példáért lásd: [trendekkel kapcsolatos témakörök a HDInsight alatt futó Apache Storm Twitter](hdinsight-storm-twitter-trending.md).</span><span class="sxs-lookup"><span data-stu-id="7e208-281">For an example of a Trident application, see [Twitter trending topics with Apache Storm on HDInsight](hdinsight-storm-twitter-trending.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e208-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7e208-282">Next Steps</span></span>

<span data-ttu-id="7e208-283">Megtanulta, hogyan toocreate a Storm-topológia Java használatával.</span><span class="sxs-lookup"><span data-stu-id="7e208-283">You have learned how toocreate a Storm topology by using Java.</span></span> <span data-ttu-id="7e208-284">Most megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="7e208-284">Now learn how to:</span></span>

* [<span data-ttu-id="7e208-285">Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="7e208-285">Deploy and manage Apache Storm topologies on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)

* [<span data-ttu-id="7e208-286">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="7e208-286">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="7e208-287">Található további példa Storm-topológiák ellátogatva [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="7e208-287">You can find more example Storm topologies by visiting [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

