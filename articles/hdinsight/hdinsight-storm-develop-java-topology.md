---
title: "Apache Storm például Java-topológiák - Azure HDInsight |} Microsoft Docs"
description: "Útmutató Apache Storm-topológiák létrehozása a Java hozzon létre egy példa a word-count topológiához."
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
ms.openlocfilehash: 36285fbaf1da3c566d338bd5612eebad327eaf50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-apache-storm-topology-in-java"></a><span data-ttu-id="9c4c0-104">Hozzon létre egy Apache Storm-topológia a Java nyelven</span><span class="sxs-lookup"><span data-stu-id="9c4c0-104">Create an Apache Storm topology in Java</span></span>

<span data-ttu-id="9c4c0-105">Útmutató az Apache Storm egy Java-alapú topológiák létrehozását is.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-105">Learn how to create a Java-based topology for Apache Storm.</span></span> <span data-ttu-id="9c4c0-106">A Storm-topológia, amely egy word-count alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-106">You create a Storm topology that implements a word-count application.</span></span> <span data-ttu-id="9c4c0-107">Maven használatával hozza létre, és a projekt csomag.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-107">You use Maven to build and package the project.</span></span> <span data-ttu-id="9c4c0-108">Ezt követően megtanulhatja a fluxus keretrendszerrel topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-108">Then, you learn how to define the topology using the Flux framework.</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-109">Fluxus keretében elérhető Storm 0.10.0-s vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-109">The Flux framework is available in Storm 0.10.0 or later.</span></span> <span data-ttu-id="9c4c0-110">A Storm 0.10.0-s HDInsight 3.3 és 3.4 érhető el.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-110">Storm 0.10.0 is available with HDInsight 3.3 and 3.4.</span></span>

<span data-ttu-id="9c4c0-111">A jelen dokumentumban leírt lépések elvégzése után a topológia telepítene a HDInsight alatt futó Apache Storm.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-111">After completing the steps in this document, you can deploy the topology to Apache Storm on HDInsight.</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-112">Ebben a dokumentumban létrehozott Storm-topológia példák befejezett verzióját [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-112">A completed version of the Storm topology examples created in this document is available at [https://github.com/Azure-Samples/hdinsight-java-storm-wordcount](https://github.com/Azure-Samples/hdinsight-java-storm-wordcount).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9c4c0-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9c4c0-113">Prerequisites</span></span>

* [<span data-ttu-id="9c4c0-114">Java fejlesztői készlet (JDK) 7-es verzió</span><span class="sxs-lookup"><span data-stu-id="9c4c0-114">Java Developer Kit (JDK) version 7</span></span>](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)

* <span data-ttu-id="9c4c0-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven rendszer project build Java-projektek esetében.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-115">[Maven (https://maven.apache.org/download.cgi)](https://maven.apache.org/download.cgi): Maven is a project build system for Java projects.</span></span>

* <span data-ttu-id="9c4c0-116">Egy szövegszerkesztőben, vagy IDE.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-116">A text editor or IDE.</span></span>

## <a name="configure-environment-variables"></a><span data-ttu-id="9c4c0-117">Környezeti változók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-117">Configure environment variables</span></span>

<span data-ttu-id="9c4c0-118">A következő környezeti változók Java és a JDK telepítésekor lehet beállítani.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-118">The following environment variables may be set when you install Java and the JDK.</span></span> <span data-ttu-id="9c4c0-119">Azonban ellenőrizni kell, hogy léteznek, illetve a rendszer a megfelelő értékeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-119">However, you should check that they exist and that they contain the correct values for your system.</span></span>

* <span data-ttu-id="9c4c0-120">**JAVA_HOME** -érdemes mutasson a mappát, ahová a Java-futtatókörnyezet (JRE) telepítve van.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-120">**JAVA_HOME** - should point to the directory where the Java runtime environment (JRE) is installed.</span></span> <span data-ttu-id="9c4c0-121">Például egy Unix vagy Linux terjesztési nem rendelkezhet hasonló érték `/usr/lib/jvm/java-7-oracle`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-121">For example, in a Unix or Linux distribution, it should have a value similar to `/usr/lib/jvm/java-7-oracle`.</span></span> <span data-ttu-id="9c4c0-122">A Windows rendszerben kellene hasonló érték`c:\Program Files (x86)\Java\jre1.7`</span><span class="sxs-lookup"><span data-stu-id="9c4c0-122">In Windows, it would have a value similar to `c:\Program Files (x86)\Java\jre1.7`</span></span>

* <span data-ttu-id="9c4c0-123">**Elérési út** -tartalmaznia kell a következő elérési utak:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-123">**PATH** - should contain the following paths:</span></span>

  * <span data-ttu-id="9c4c0-124">**JAVA_HOME** (vagy ezzel egyenértékű elérési útja)</span><span class="sxs-lookup"><span data-stu-id="9c4c0-124">**JAVA_HOME** (or the equivalent path)</span></span>

  * <span data-ttu-id="9c4c0-125">**JAVA_HOME\bin** (vagy ezzel egyenértékű elérési útja)</span><span class="sxs-lookup"><span data-stu-id="9c4c0-125">**JAVA_HOME\bin** (or the equivalent path)</span></span>

  * <span data-ttu-id="9c4c0-126">A mappát, ahová a Maven telepítve van</span><span class="sxs-lookup"><span data-stu-id="9c4c0-126">The directory where Maven is installed</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="9c4c0-127">Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-127">Create a Maven project</span></span>

<span data-ttu-id="9c4c0-128">A parancssorból az alábbi parancs segítségével nevű Maven-projekt létrehozása **WordCount**:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-128">From the command line, use the following command to create a Maven project named **WordCount**:</span></span>

```bash
mvn archetype:generate -DarchetypeArtifactId=maven-archetype-quickstart -DgroupId=com.microsoft.example -DartifactId=WordCount -DinteractiveMode=false
```

> [!NOTE]
> <span data-ttu-id="9c4c0-129">Ha a PowerShell használata esetén meg kell helyezze a`-D` az idézőjelek közé foglalt paraméterek.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-129">If you are using PowerShell, you must surround the`-D` parameters with double quotes.</span></span>
>
> `mvn archetype:generate "-DarchetypeArtifactId=maven-archetype-quickstart" "-DgroupId=com.microsoft.example" "-DartifactId=WordCount" "-DinteractiveMode=false"`

<span data-ttu-id="9c4c0-130">Ez a parancs létrehoz egy könyvtárat nevű `WordCount` az aktuális helyen, amely tartalmazza egy alapszintű Maven project.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-130">This command creates a directory named `WordCount` at the current location, which contains a basic Maven project.</span></span> <span data-ttu-id="9c4c0-131">A `WordCount` könyvtár a következő elemeket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-131">The `WordCount` directory contains the following items:</span></span>

* <span data-ttu-id="9c4c0-132">`pom.xml`: A Maven project beállításait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-132">`pom.xml`: Contains settings for the Maven project.</span></span>
* <span data-ttu-id="9c4c0-133">`src\main\java\com\microsoft\example`: Az alkalmazás kódját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-133">`src\main\java\com\microsoft\example`: Contains your application code.</span></span>
* <span data-ttu-id="9c4c0-134">`src\test\java\com\microsoft\example`: Az alkalmazás a tesztek tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-134">`src\test\java\com\microsoft\example`: Contains tests for your application.</span></span> 

### <a name="remove-the-generated-example-code"></a><span data-ttu-id="9c4c0-135">A generált kód eltávolítása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-135">Remove the generated example code</span></span>

<span data-ttu-id="9c4c0-136">A generált vizsgálat és az alkalmazásfájlok törlése:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-136">Delete the generated test and the application files:</span></span>

* <span data-ttu-id="9c4c0-137">**src\test\java\com\microsoft\example\AppTest.Java**</span><span class="sxs-lookup"><span data-stu-id="9c4c0-137">**src\test\java\com\microsoft\example\AppTest.java**</span></span>
* <span data-ttu-id="9c4c0-138">**src\main\java\com\microsoft\example\App.Java**</span><span class="sxs-lookup"><span data-stu-id="9c4c0-138">**src\main\java\com\microsoft\example\App.java**</span></span>

## <a name="add-maven-repositories"></a><span data-ttu-id="9c4c0-139">Adja hozzá a Maven tárházak</span><span class="sxs-lookup"><span data-stu-id="9c4c0-139">Add Maven repositories</span></span>

<span data-ttu-id="9c4c0-140">HDInsight Hortonworks Data Platform (HDP) alkalmazásban, alapul, ezért azt javasoljuk, töltse le a függőségek az Apache Storm-projektek a Hortonworks tárház használatával.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-140">HDInsight is based on the Hortonworks Data Platform (HDP), so we recommend using the Hortonworks repository to download dependencies for your Apache Storm projects.</span></span> <span data-ttu-id="9c4c0-141">Az a __pom.xml__ fájlban adja hozzá a következő XML-kód után a `<url>http://maven.apache.org</url>` sor:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-141">In the __pom.xml__ file, add the following XML after the `<url>http://maven.apache.org</url>` line:</span></span>

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

## <a name="add-properties"></a><span data-ttu-id="9c4c0-142">Tulajdonságok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-142">Add properties</span></span>

<span data-ttu-id="9c4c0-143">Maven lehetővé teszi tulajdonságok nevű projektszintű értékek adhatók meg.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-143">Maven allows you to define project-level values called properties.</span></span> <span data-ttu-id="9c4c0-144">Az a __pom.xml__, adja hozzá a következő karakterlánc után a `</repositories>` sor:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-144">In the __pom.xml__, add the following text after the `</repositories>` line:</span></span>

```xml
<properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--
    This is a version of Storm from the Hortonworks repository that is compatible with HDInsight.
    -->
    <storm.version>1.0.1.2.5.3.0-37</storm.version>
</properties>
```

<span data-ttu-id="9c4c0-145">Ezután már használhatja ezt az értéket a többi szakasza pedig a `pom.xml`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-145">You can now use this value in other sections of the `pom.xml`.</span></span> <span data-ttu-id="9c4c0-146">Például a Storm összetevőiről verzióját meghatározásakor használhatja `${storm.version}` rögzített kódolási érték helyett.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-146">For example, when specifying the version of Storm components, you can use `${storm.version}` instead of hard coding a value.</span></span>

## <a name="add-dependencies"></a><span data-ttu-id="9c4c0-147">Adja hozzá a függőségek</span><span class="sxs-lookup"><span data-stu-id="9c4c0-147">Add dependencies</span></span>

<span data-ttu-id="9c4c0-148">Vegyen fel egy függőséget Storm-összetevőket.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-148">Add a dependency for Storm components.</span></span> <span data-ttu-id="9c4c0-149">Nyissa meg a `pom.xml` fájlt, és adja hozzá a következő kódot a `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-149">Open the `pom.xml` file and add the following code in the `<dependencies>` section:</span></span>

```xml
<dependency>
    <groupId>org.apache.storm</groupId>
    <artifactId>storm-core</artifactId>
    <version>${storm.version}</version>
    <!-- keep storm out of the jar-with-dependencies -->
    <scope>provided</scope>
</dependency>
```

<span data-ttu-id="9c4c0-150">Fordítási időben Maven használja ezt az információt kereséséhez `storm-core` a Maven-tárházban.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-150">At compile time, Maven uses this information to look up `storm-core` in the Maven repository.</span></span> <span data-ttu-id="9c4c0-151">Először a jelek a tárházban a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-151">It first looks in the repository on your local computer.</span></span> <span data-ttu-id="9c4c0-152">Ha a fájlokat nem létezik, a Maven letölti azokat a nyilvános Maven tárházból, és tárolja őket a helyi tárházban.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-152">If the files aren't there, Maven downloads them from the public Maven repository and stores them in the local repository.</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-153">Figyelje meg a `<scope>provided</scope>` sor ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-153">Notice the `<scope>provided</scope>` line in this section.</span></span> <span data-ttu-id="9c4c0-154">Ez a beállítás közli kizárandó Maven **storm-core** bármely JAR fájlok jönnek létre, mert azt a rendszer biztosítja.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-154">This setting tells Maven to exclude **storm-core** from any JAR files that are created, because it is provided by the system.</span></span>

## <a name="build-configuration"></a><span data-ttu-id="9c4c0-155">Konfiguráció</span><span class="sxs-lookup"><span data-stu-id="9c4c0-155">Build configuration</span></span>

<span data-ttu-id="9c4c0-156">Maven beépülő modulok lehetővé teszik a létrehozási szakaszokra a projekt testreszabása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-156">Maven plug-ins allow you to customize the build stages of the project.</span></span> <span data-ttu-id="9c4c0-157">Például hogyan fordítását, akkor a projekt vagy csomagolása, a JAR-fájlra.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-157">For example, how the project is compiled or how to package it into a JAR file.</span></span> <span data-ttu-id="9c4c0-158">Nyissa meg a `pom.xml` fájlt, és adja hozzá a következő kódot közvetlenül a fenti a `</project>` sor.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-158">Open the `pom.xml` file and add the following code directly above the `</project>` line.</span></span>

```xml
<build>
    <plugins>
    </plugins>
    <resources>
    </resources>
</build>
```

<span data-ttu-id="9c4c0-159">Ez a szakasz segítségével adja hozzá a beépülő modulok, erőforrások és egyéb build-konfigurációs beállítások.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-159">This section is used to add plug-ins, resources, and other build configuration options.</span></span> <span data-ttu-id="9c4c0-160">A teljes körű referenciáért a **pom.xml** fájl című [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-160">For a full reference of the **pom.xml** file, see [http://maven.apache.org/pom.html](http://maven.apache.org/pom.html).</span></span>

### <a name="add-plug-ins"></a><span data-ttu-id="9c4c0-161">Beépülő modulok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-161">Add plug-ins</span></span>

<span data-ttu-id="9c4c0-162">Az Apache Storm-topológiák megvalósított Java a [Exec Maven beépülő modul](http://www.mojohaus.org/exec-maven-plugin/) akkor hasznos, mivel lehetővé teszi a topológia könnyen helyi futtatásához a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-162">For Apache Storm topologies implemented in Java, the [Exec Maven Plugin](http://www.mojohaus.org/exec-maven-plugin/) is useful because it allows you to easily run the topology locally in your development environment.</span></span> <span data-ttu-id="9c4c0-163">Adja hozzá a következőt a `<plugins>` szakasza a `pom.xml` a beépülő modul Exec Maven-fájl:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-163">Add the following to the `<plugins>` section of the `pom.xml` file to include the Exec Maven plugin:</span></span>

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

<span data-ttu-id="9c4c0-164">Egy másik hasznos beépülő modul a [Apache Maven fordító beépülő modul](http://maven.apache.org/plugins/maven-compiler-plugin/), amellyel fordítási beállítások módosítása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-164">Another useful plug-in is the [Apache Maven Compiler Plugin](http://maven.apache.org/plugins/maven-compiler-plugin/), which is used to change compilation options.</span></span> <span data-ttu-id="9c4c0-165">A módosítások a Java verzióját használó Maven a forrás és cél az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-165">The changes the Java version that Maven uses for the source and target for your application.</span></span>

* <span data-ttu-id="9c4c0-166">A HDInsight __3.4 vagy korábbi__, állítsa be a forrás és cél a Java-verziót __1.7__.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-166">For HDInsight __3.4 or earlier__, set the source and target Java version to __1.7__.</span></span>

* <span data-ttu-id="9c4c0-167">A HDInsight __3.5__, állítsa be a forrás és cél a Java-verziót __1.8__.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-167">For HDInsight __3.5__, set the source and target Java version to __1.8__.</span></span>

<span data-ttu-id="9c4c0-168">Adja hozzá a következő szöveget a `<plugins>` szakasza a `pom.xml` fájlt a az Apache Maven fordító beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-168">Add the following text in the `<plugins>` section of the `pom.xml` file to include the Apache Maven Compiler plugin.</span></span> <span data-ttu-id="9c4c0-169">Ez a példa 1.8, határozza meg, így a célverzió HDInsight 3.5-ös verzióját.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-169">This example specifies 1.8, so the target HDInsight version is 3.5.</span></span>

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

### <a name="configure-resources"></a><span data-ttu-id="9c4c0-170">Erőforrások konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-170">Configure resources</span></span>

<span data-ttu-id="9c4c0-171">Az erőforrások szakasz lehetővé teszi, hogy nem kód erőforrások, például a topológia összetevők által igényelt konfigurációs fájlokat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-171">The resources section allows you to include non-code resources such as configuration files needed by components in the topology.</span></span> <span data-ttu-id="9c4c0-172">Ennél a példánál adja hozzá a következő szöveget a `<resources>` szakaszában a "pom.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-172">For this example, add the following text in the `<resources>` section of the \`pom.xml file.</span></span>

```xml
<resource>
    <directory>${basedir}/resources</directory>
    <filtering>false</filtering>
    <includes>
        <include>log4j2.xml</include>
    </includes>
</resource>
```

<span data-ttu-id="9c4c0-173">Ebben a példában az erőforrások könyvtárat ad a projekt gyökérkönyvtárában (`${basedir}`) olyan erőforrásokat tartalmaz, és a fájlt tartalmazó helyként `log4j2.xml`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-173">This example adds the resources directory in the root of the project (`${basedir}`) as a location that contains resources, and includes the file named `log4j2.xml`.</span></span> <span data-ttu-id="9c4c0-174">Ez a fájl által a topológia a rendszer milyen információkat naplózza konfigurálására szolgál.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-174">This file is used to configure what information is logged by the topology.</span></span>

## <a name="create-the-topology"></a><span data-ttu-id="9c4c0-175">A topológia létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-175">Create the topology</span></span>

<span data-ttu-id="9c4c0-176">A Java-alapú Apache Storm-topológia áll meg kell írni három összetevő (vagy hivatkozás) a függőség beállításához.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-176">A Java-based Apache Storm topology consists of three components that you must author (or reference) as a dependency.</span></span>

* <span data-ttu-id="9c4c0-177">**Spoutok**: olvassa be a külső adatokat adatforrásokat, és megfelelően kibocsát adatstreamek azokat a topológia.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-177">**Spouts**: Reads data from external sources and emits streams of data into the topology.</span></span>

* <span data-ttu-id="9c4c0-178">**Boltok**: feldolgozási végez spoutokkal kapcsolatban, vagy más boltokhoz által kibocsátott adatfolyamokat, és megfelelően kibocsát egy vagy több adatfolyamokat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-178">**Bolts**: Performs processing on streams emitted by spouts or other bolts, and emits one or more streams.</span></span>

* <span data-ttu-id="9c4c0-179">**Topológia**: hogyan a spoutokkal kapcsolatban és boltokhoz vannak rendezve, és a belépési pontot nyújt az topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-179">**Topology**: Defines how the spouts and bolts are arranged, and provides the entry point for the topology.</span></span>

### <a name="create-the-spout"></a><span data-ttu-id="9c4c0-180">A spout létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-180">Create the spout</span></span>

<span data-ttu-id="9c4c0-181">Külső adatforrások beállításával kapcsolatos követelmények csökkentése érdekében a következő spout egyszerűen véletlenszerű mondat bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-181">To reduce requirements for setting up external data sources, the following spout simply emits random sentences.</span></span> <span data-ttu-id="9c4c0-182">A mellékelt spout módosított változatát a [Storm-kezdőpéldák](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-182">It is a modified version of a spout that is provided with the [Storm-Starter examples](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter).</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-183">Példa egy egy külső adatforrásból olvasó spout tekintse meg az alábbi példák egyikét:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-183">For an example of a spout that reads from an external data source, see one of the following examples:</span></span>
>
> * <span data-ttu-id="9c4c0-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): egy példa spout, amely Twitter olvassa be</span><span class="sxs-lookup"><span data-stu-id="9c4c0-184">[TwitterSampleSPout](https://github.com/apache/storm/blob/0.10.x-branch/examples/storm-starter/src/jvm/storm/starter/spout/TwitterSampleSpout.java): An example spout that reads from Twitter</span></span>
> * <span data-ttu-id="9c4c0-185">[A Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): egy spout, amely Kafka olvassa be</span><span class="sxs-lookup"><span data-stu-id="9c4c0-185">[Storm-Kafka](https://github.com/apache/storm/tree/0.10.x-branch/external/storm-kafka): A spout that reads from Kafka</span></span>

<span data-ttu-id="9c4c0-186">Hozzon létre egy fájlt a spout `RandomSentenceSpout.java` a a `src\main\java\com\microsoft\example` könyvtárra, és használja a következő Java kód tartalma:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-186">For the spout, create a file named `RandomSentenceSpout.java` in the `src\main\java\com\microsoft\example` directory and use the following Java code as the contents:</span></span>

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
  //Collector used to emit output
  SpoutOutputCollector _collector;
  //Used to generate a random number
  Random _rand;

  //Open is called when an instance of the class is created
  @Override
  public void open(Map conf, TopologyContext context, SpoutOutputCollector collector) {
  //Set the instance collector to the one passed in
    _collector = collector;
    //For randomness
    _rand = new Random();
  }

  //Emit data to the stream
  @Override
  public void nextTuple() {
  //Sleep for a bit
    Utils.sleep(100);
    //The sentences that are randomly emitted
    String[] sentences = new String[]{ "the cow jumped over the moon", "an apple a day keeps the doctor away",
        "four score and seven years ago", "snow white and the seven dwarfs", "i am at two with nature" };
    //Randomly pick a sentence
    String sentence = sentences[_rand.nextInt(sentences.length)];
    //Emit the sentence
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

  //Declare the output fields. In this case, an sentence
  @Override
  public void declareOutputFields(OutputFieldsDeclarer declarer) {
    declarer.declare(new Fields("sentence"));
  }
}
```

> [!NOTE]
> <span data-ttu-id="9c4c0-187">Bár ez a topológia csak egy spout, mások is rendelkezhet, amely adatokat különböző forrásokból történő a topológia több.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-187">Although this topology uses only one spout, others may have several that feed data from different sources into the topology.</span></span>

### <a name="create-the-bolts"></a><span data-ttu-id="9c4c0-188">A boltokhoz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-188">Create the bolts</span></span>

<span data-ttu-id="9c4c0-189">Szögek kezelni az adatok feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-189">Bolts handle the data processing.</span></span> <span data-ttu-id="9c4c0-190">Ez a topológia két boltokhoz használja:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-190">This topology uses two bolts:</span></span>

* <span data-ttu-id="9c4c0-191">**SplitSentence**: felosztja a mondatok által kibocsátott **RandomSentenceSpout** az egyes szavakat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-191">**SplitSentence**: Splits the sentences emitted by **RandomSentenceSpout** into individual words.</span></span>

* <span data-ttu-id="9c4c0-192">**WordCount**: hányszor történt minden szó található.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-192">**WordCount**: Counts how many times each word has occurred.</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-193">Szögek is végrehajthat, például számítási, adatmegőrzés vagy a külső összetevőkre van szó.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-193">Bolts can do anything, for example, computation, persistence, or talking to external components.</span></span>

<span data-ttu-id="9c4c0-194">Hozzon létre két új fájl `SplitSentence.java` és `WordCount.java` a a `src\main\java\com\microsoft\example` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-194">Create two new files, `SplitSentence.java` and `WordCount.java` in the `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="9c4c0-195">A fájlok használata a tartalom a következő szöveget:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-195">Use the following text as the contents for the files:</span></span>

#### <a name="splitsentence"></a><span data-ttu-id="9c4c0-196">SplitSentence</span><span class="sxs-lookup"><span data-stu-id="9c4c0-196">SplitSentence</span></span>

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

  //Execute is called to process tuples
  @Override
  public void execute(Tuple tuple, BasicOutputCollector collector) {
    //Get the sentence content from the tuple
    String sentence = tuple.getString(0);
    //An iterator to get each word
    BreakIterator boundary=BreakIterator.getWordInstance();
    //Give the iterator the sentence
    boundary.setText(sentence);
    //Find the beginning first word
    int start=boundary.first();
    //Iterate over each word and emit it to the output stream
    for (int end=boundary.next(); end != BreakIterator.DONE; start=end, end=boundary.next()) {
      //get the word
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

#### <a name="wordcount"></a><span data-ttu-id="9c4c0-197">WordCount</span><span class="sxs-lookup"><span data-stu-id="9c4c0-197">WordCount</span></span>

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
  //How often to emit a count of words
  private Integer emitFrequency;

  // Default constructor
  public WordCount() {
      emitFrequency=5; // Default to 60 seconds
  }

  // Constructor that sets emit frequency
  public WordCount(Integer frequency) {
      emitFrequency=frequency;
  }

  //Configure frequency of tick tuples for this bolt
  //This delivers a 'tick' tuple on a specific interval,
  //which is used to trigger certain actions
  @Override
  public Map<String, Object> getComponentConfiguration() {
      Config conf = new Config();
      conf.put(Config.TOPOLOGY_TICK_TUPLE_FREQ_SECS, emitFrequency);
      return conf;
  }

  //execute is called to process tuples
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
      //Get the word contents from the tuple
      String word = tuple.getString(0);
      //Have we counted any already?
      Integer count = counts.get(word);
      if (count == null)
        count = 0;
      //Increment the count and store it
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

### <a name="define-the-topology"></a><span data-ttu-id="9c4c0-198">A topológia meghatározása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-198">Define the topology</span></span>

<span data-ttu-id="9c4c0-199">A topológia kötelékek a spoutokkal kapcsolatban, és a grafikon, amely meghatározza, hogyan közötti adatáramlás a összetevők együttesen boltok.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-199">The topology ties the spouts and bolts together into a graph, which defines how data flows between the components.</span></span> <span data-ttu-id="9c4c0-200">Párhuzamossági mutatók Storm használó a fürtön belül összetevők példányai létrehozásakor is tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-200">It also provides parallelism hints that Storm uses when creating instances of the components within the cluster.</span></span>

<span data-ttu-id="9c4c0-201">Az alábbi képen a grafikon az ebben a topológiában az összetevők egyszerű diagram.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-201">The following image is a basic diagram of the graph of components for this topology.</span></span>

![a spoutokkal kapcsolatban és boltokhoz elrendezéssel bemutató ábra](./media/hdinsight-storm-develop-java-topology/wordcount-topology.png)

<span data-ttu-id="9c4c0-203">A topológia alkalmazásához hozzon létre egy fájlt `WordCountTopology.java` a a `src\main\java\com\microsoft\example` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-203">To implement the topology, create a file named `WordCountTopology.java` in the `src\main\java\com\microsoft\example` directory.</span></span> <span data-ttu-id="9c4c0-204">A fájl tartalmát az alábbira Java használata:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-204">Use the following Java code as the contents of the file:</span></span>

```java
package com.microsoft.example;

import org.apache.storm.Config;
import org.apache.storm.LocalCluster;
import org.apache.storm.StormSubmitter;
import org.apache.storm.topology.TopologyBuilder;
import org.apache.storm.tuple.Fields;

import com.microsoft.example.RandomSentenceSpout;

public class WordCountTopology {

  //Entry point for the topology
  public static void main(String[] args) throws Exception {
  //Used to build the topology
    TopologyBuilder builder = new TopologyBuilder();
    //Add the spout, with a name of 'spout'
    //and parallelism hint of 5 executors
    builder.setSpout("spout", new RandomSentenceSpout(), 5);
    //Add the SplitSentence bolt, with a name of 'split'
    //and parallelism hint of 8 executors
    //shufflegrouping subscribes to the spout, and equally distributes
    //tuples (sentences) across instances of the SplitSentence bolt
    builder.setBolt("split", new SplitSentence(), 8).shuffleGrouping("spout");
    //Add the counter, with a name of 'count'
    //and parallelism hint of 12 executors
    //fieldsgrouping subscribes to the split bolt, and
    //ensures that the same word is sent to the same instance (group by field 'word')
    builder.setBolt("count", new WordCount(), 12).fieldsGrouping("split", new Fields("word"));

    //new configuration
    Config conf = new Config();
    //Set to false to disable debug information when
    // running in production on a cluster
    conf.setDebug(false);

    //If there are arguments, we are running on a cluster
    if (args != null && args.length > 0) {
      //parallelism hint to set the number of workers
      conf.setNumWorkers(3);
      //submit the topology
      StormSubmitter.submitTopology(args[0], conf, builder.createTopology());
    }
    //Otherwise, we are running locally
    else {
      //Cap the maximum number of executors that can be spawned
      //for a component to 3
      conf.setMaxTaskParallelism(3);
      //LocalCluster is used to run locally
      LocalCluster cluster = new LocalCluster();
      //submit the topology
      cluster.submitTopology("word-count", conf, builder.createTopology());
      //sleep
      Thread.sleep(10000);
      //shut down the cluster
      cluster.shutdown();
    }
  }
}
```

### <a name="configure-logging"></a><span data-ttu-id="9c4c0-205">Naplózás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9c4c0-205">Configure logging</span></span>

<span data-ttu-id="9c4c0-206">A Storm az Apache Log4j használatával naplózza az információkat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-206">Storm uses Apache Log4j to log information.</span></span> <span data-ttu-id="9c4c0-207">Ha nem konfigurálja a naplózást, a topológia diagnosztikai adatokat bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-207">If you do not configure logging, the topology emits diagnostic information.</span></span> <span data-ttu-id="9c4c0-208">Szabályozhatja, hogy mi a naplózására akkor kerül sor, hozzon létre egy fájlt `log4j2.xml` a a `resources` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-208">To control what is logged, create a file named `log4j2.xml` in the `resources` directory.</span></span> <span data-ttu-id="9c4c0-209">Az alábbi XML-fájl használata a fájl tartalmát.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-209">Use the following XML as the contents of the file.</span></span>

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

<span data-ttu-id="9c4c0-210">Az XML számára egy új naplózó konfigurálja a `com.microsoft.example` osztályt, amely ebben a példában topológiában a összetevőket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-210">This XML configures a new logger for the `com.microsoft.example` class, which includes the components in this example topology.</span></span> <span data-ttu-id="9c4c0-211">A szintje nyomkövetési a a tranzakciónaplókat tartalmazó, amely minden ebben a topológiában-összetevők által kibocsátott naplózási információkat rögzíti.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-211">The level is set to trace for this logger, which captures any logging information emitted by components in this topology.</span></span>

<span data-ttu-id="9c4c0-212">A `<Root level="error">` szakasz konfigurálja a naplózási gyökérszinten (mindent nem szereplő `com.microsoft.example`) csak a hibák naplózása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-212">The `<Root level="error">` section configures the root level of logging (everything not in `com.microsoft.example`) to only log error information.</span></span>

<span data-ttu-id="9c4c0-213">Log4j naplózásának konfigurálásáról további információkért lásd: [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-213">For more information on configuring logging for Log4j, see [http://logging.apache.org/log4j/2.x/manual/configuration.html](http://logging.apache.org/log4j/2.x/manual/configuration.html).</span></span>

> [!NOTE]
> <span data-ttu-id="9c4c0-214">Storm verzióját 0.10.0-s és magasabb használata Log4j 2.x.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-214">Storm version 0.10.0 and higher use Log4j 2.x.</span></span> <span data-ttu-id="9c4c0-215">A storm régebbi verzióit használja Log4j 1.x, a naplózási konfiguráció más formátumú használt.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-215">Older versions of storm used Log4j 1.x, which used a different format for log configuration.</span></span> <span data-ttu-id="9c4c0-216">A régebbi konfigurációtól tudnivalókért lásd: [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-216">For information on the older configuration, see [http://wiki.apache.org/logging-log4j/Log4jXmlFormat](http://wiki.apache.org/logging-log4j/Log4jXmlFormat).</span></span>

## <a name="test-the-topology-locally"></a><span data-ttu-id="9c4c0-217">A topológia helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="9c4c0-217">Test the topology locally</span></span>

<span data-ttu-id="9c4c0-218">A fájlok mentése után a következő paranccsal helyileg a topológia teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-218">After you save the files, use the following command to test the topology locally.</span></span>

```bash
mvn compile exec:java -Dstorm.topology=com.microsoft.example.WordCountTopology
```

<span data-ttu-id="9c4c0-219">Futtatja, a topológia indítási információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-219">As it runs, the topology displays startup information.</span></span> <span data-ttu-id="9c4c0-220">A következő szöveget a word-count kimeneti példája:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-220">The following text is an example of the word count output:</span></span>

    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
    17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
    17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs
    17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word snow

<span data-ttu-id="9c4c0-221">Ez a példa napló azt jelzi, hogy a word "és" 113 alkalommal kibocsátását.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-221">This example log indicates that the word 'and' has been emitted 113 times.</span></span> <span data-ttu-id="9c4c0-222">A szám továbbra is, amíg a topológia fut, mert a spout folyamatosan ugyanazt a mondatok bocsát ki.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-222">The count continues to go up as long as the topology runs because the spout continuously emits the same sentences.</span></span>

<span data-ttu-id="9c4c0-223">Egy 5 másodperces időköze van szó kibocsátási és a számok között.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-223">There is a 5-second interval between emission of words and counts.</span></span> <span data-ttu-id="9c4c0-224">A **WordCount** összetevő adatok csak létrehozása egy rekordot érkezésekor van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-224">The **WordCount** component is configured to only emit information when a tick tuple arrives.</span></span> <span data-ttu-id="9c4c0-225">Adott listának csak kézbesítési öt másodpercenként osztásjelek kéri.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-225">It requests that tick tuples are only delivered every five seconds.</span></span>

## <a name="convert-the-topology-to-flux"></a><span data-ttu-id="9c4c0-226">A topológia átalakítása fluxus</span><span class="sxs-lookup"><span data-stu-id="9c4c0-226">Convert the topology to Flux</span></span>

<span data-ttu-id="9c4c0-227">Fluxus egy új keretrendszer elérhető Storm 0.10.0-s vagy újabb, amely lehetővé teszi a megvalósítás konfigurációja külön.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-227">Flux is a new framework available with Storm 0.10.0 and higher, which allows you to separate configuration from implementation.</span></span> <span data-ttu-id="9c4c0-228">Az összetevők továbbra is Java vannak definiálva, de a topológia definíciója YAM-fájllal.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-228">Your components are still defined in Java, but the topology is defined using a YAML file.</span></span> <span data-ttu-id="9c4c0-229">Csomagdefiníció egy alapértelmezett topológia a projektet, vagy egy önálló fájlt használja, amikor a topológiához.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-229">You can package a default topology definition with your project, or use a standalone file when submitting the topology.</span></span> <span data-ttu-id="9c4c0-230">A topológia a Storm elküldésekor segítségével környezeti változókat vagy konfigurációs fájlok feltöltése az YAM-topológia definícióban szereplő értékek.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-230">When submitting the topology to Storm, you can use environment variables or configuration files to populate values in the YAML topology definition.</span></span>

<span data-ttu-id="9c4c0-231">A YAM fájl határozza meg a topológia és az adatok az összetevők közötti folyamat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-231">The YAML file defines the components to use for the topology and the data flow between them.</span></span> <span data-ttu-id="9c4c0-232">Megadhat egy YAM-fájl részeként a jar-fájlra, vagy egy külső YAM-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-232">You can include a YAML file as part of the jar file or you can use an external YAML file.</span></span>

<span data-ttu-id="9c4c0-233">Fluxus további információkért lásd: [fluxus keretrendszer (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-233">For more information on Flux, see [Flux framework (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

> [!WARNING]
> <span data-ttu-id="9c4c0-234">Oka az, hogy egy [hiba (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) Storm 1.0.1-es, előfordulhat, hogy telepíteni szeretné a [Storm fejlesztőkörnyezet](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) fluxus topológiák helyi futtatásához.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-234">Due to a [bug (https://issues.apache.org/jira/browse/STORM-2055)](https://issues.apache.org/jira/browse/STORM-2055) with Storm 1.0.1, you may need to install a [Storm development environment](https://storm.apache.org/releases/1.0.1/Setting-up-development-environment.html) to run Flux topologies locally.</span></span>

1. <span data-ttu-id="9c4c0-235">Helyezze át a `WordCountTopology.java` fájlt a projekt kívüli.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-235">Move the `WordCountTopology.java` file out of the project.</span></span> <span data-ttu-id="9c4c0-236">Korábban ezt a fájlt a topológia definiálva, de a fluxus nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-236">Previously, this file defined the topology, but isn't needed with Flux.</span></span>

2. <span data-ttu-id="9c4c0-237">Az a `resources` könyvtár, hozzon létre egy fájlt `topology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-237">In the `resources` directory, create a file named `topology.yaml`.</span></span> <span data-ttu-id="9c4c0-238">Ez a fájl tartalmát a következő szöveg használható.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-238">Use the following text as the contents of this file.</span></span>

        name: "wordcount"       # friendly name for the topology
        
        config:                 # Topology configuration
        topology.workers: 1     # Hint for the number of workers to create
        
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
            from: "sentence-spout"       # The stream emitter
            to: "splitter-bolt"          # The stream consumer
            grouping:                    # Grouping type
                type: SHUFFLE
          
            - name: "Splitter -> Counter"
            from: "splitter-bolt"
            to: "counter-bolt"
            grouping:
            type: FIELDS
                args: ["word"]           # field(s) to group on

3. <span data-ttu-id="9c4c0-239">A következő módosításokat a `pom.xml` fájlt.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-239">Make the following changes to the `pom.xml` file.</span></span>
   
   * <span data-ttu-id="9c4c0-240">Adja hozzá a következő új függőséghez a `<dependencies>` szakasz:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-240">Add the following new dependency in the `<dependencies>` section:</span></span>
     
        ```xml
        <!-- Add a dependency on the Flux framework -->
        <dependency>
            <groupId>org.apache.storm</groupId>
            <artifactId>flux-core</artifactId>
            <version>${storm.version}</version>
        </dependency>
        ```
   * <span data-ttu-id="9c4c0-241">Adja hozzá a következő beépülő modult a `<plugins>` szakasz.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-241">Add the following plugin to the `<plugins>` section.</span></span> <span data-ttu-id="9c4c0-242">A beépülő modul a létrehozása a projekthez (jar-fájlt) csomag kezeli, és néhány meghatározott átalakítások fluxus alkalmazza, ha a csomag létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-242">This plugin handles the creation of a package (jar file) for the project, and applies some transformations specific to Flux when creating the package.</span></span>
     
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
                    <!-- We're using Flux, so refer to it as main -->
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

   * <span data-ttu-id="9c4c0-243">Az a **exec-maven-beépülő modul** `<configuration>` területen módosítsa az értéket a `<mainClass>` való `org.apache.storm.flux.Flux`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-243">In the **exec-maven-plugin** `<configuration>` section, change the value for `<mainClass>` to `org.apache.storm.flux.Flux`.</span></span> <span data-ttu-id="9c4c0-244">Ez a beállítás lehetővé teszi, hogy a kezelni a topológia helyben fut a fejlesztési fluxus.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-244">This setting allows Flux to handle running the topology locally in development.</span></span>

   * <span data-ttu-id="9c4c0-245">Az a `<resources>` területen írja be a következőt a `<includes>`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-245">In the `<resources>` section, add the following to the `<includes>`.</span></span> <span data-ttu-id="9c4c0-246">Az XML-kód tartalmazza a YAM fájlt határozza meg a topológia a projekt részeként.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-246">This XML includes the YAML file that defines the topology as part of the project.</span></span>

        ```xml
        <include>topology.yaml</include>
        ```

## <a name="test-the-flux-topology-locally"></a><span data-ttu-id="9c4c0-247">A fluxus topológia helyi tesztelése</span><span class="sxs-lookup"><span data-stu-id="9c4c0-247">Test the flux topology locally</span></span>

1. <span data-ttu-id="9c4c0-248">Használja a következő fordításához és a fluxus topológia Maven használatával hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-248">Use the following to compile and execute the Flux topology using Maven:</span></span>

    ```bash
    mvn compile exec:java -Dexec.args="--local -R /topology.yaml"
    ```

    <span data-ttu-id="9c4c0-249">Ha a PowerShell használata esetén a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-249">If you are using PowerShell, use the following command:</span></span>

    ```bash
    mvn compile exec:java "-Dexec.args=--local -R /topology.yaml"
    ```

    > [!WARNING]
    > <span data-ttu-id="9c4c0-250">Ha a topológia a Storm 1.0.1-es bits használ, ez a parancs sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-250">If your topology uses Storm 1.0.1 bits, this command fails.</span></span> <span data-ttu-id="9c4c0-251">Ez a hiba oka [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-251">This failure is caused by [https://issues.apache.org/jira/browse/STORM-2055](https://issues.apache.org/jira/browse/STORM-2055).</span></span> <span data-ttu-id="9c4c0-252">Ehelyett [Storm a fejlesztési környezet telepítése](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) és az alábbi információkat.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-252">Instead, [install Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html) and use the following information.</span></span>

    <span data-ttu-id="9c4c0-253">Ha rendelkezik [Storm a fejlesztési környezetben telepített](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), ehelyett a következő parancsokat használhatja:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-253">If you have [installed Storm in your development environment](http://storm.apache.org/releases/0.10.0/Setting-up-development-environment.html), you can use the following commands instead:</span></span>

    ```bash
    mvn compile package
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local -R /topology.yaml
    ```

    <span data-ttu-id="9c4c0-254">A `--local` paraméter fut a topológia helyi módban a fejlesztési környezetet.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-254">The `--local` parameter runs the topology in local mode on your development environment.</span></span> <span data-ttu-id="9c4c0-255">A `-R /topology.yaml` paramétert használja a `topology.yaml` erőforrás fájlt a jar-fájlra a topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-255">The `-R /topology.yaml` parameter uses the `topology.yaml` file resource from the jar file to define the topology.</span></span>

    <span data-ttu-id="9c4c0-256">Futtatja, a topológia indítási információit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-256">As it runs, the topology displays startup information.</span></span> <span data-ttu-id="9c4c0-257">A következő szöveget a kimeneti példája:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-257">The following text is an example of the output:</span></span>

        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word snow
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 56 for word white
        17:33:27 [Thread-12-count] INFO  com.microsoft.example.WordCount - Emitting a count of 112 for word seven
        17:33:27 [Thread-16-count] INFO  com.microsoft.example.WordCount - Emitting a count of 195 for word the
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 113 for word and
        17:33:27 [Thread-30-count] INFO  com.microsoft.example.WordCount - Emitting a count of 57 for word dwarfs

    <span data-ttu-id="9c4c0-258">Van egy 10 másodperces késleltetési kötegek naplózott adatok között.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-258">There is a 10-second delay between batches of logged information.</span></span>

2. <span data-ttu-id="9c4c0-259">Másolatot készít a `topology.yaml` fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-259">Make a copy of the `topology.yaml` file from the project.</span></span> <span data-ttu-id="9c4c0-260">Nevezze el az új fájlt `newtopology.yaml`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-260">Name the new file `newtopology.yaml`.</span></span> <span data-ttu-id="9c4c0-261">Az a `newtopology.yaml` fájlt, keresse meg a következő szakaszt, és módosítsa az értéket a `10` való `5`.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-261">In the `newtopology.yaml` file, find the following section and change the value of `10` to `5`.</span></span> <span data-ttu-id="9c4c0-262">Ez a módosítás a kibocsátás kötegekben word számok 10 másodperc 5 közötti távolság változik.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-262">This modification changes the interval between emitting batches of word counts from 10 seconds to 5.</span></span>

    ```yaml
    - id: "counter-bolt"
    className: "com.microsoft.example.WordCount"
    constructorArgs:
    - 5
    parallelism: 1
    ```yaml

3. To run the topology, use the following command:

    ```bash
    mvn exec:java -Dexec.args="--local /path/to/newtopology.yaml"
    ```

    <span data-ttu-id="9c4c0-263">Vagy ha a fejlesztési környezet Storm rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-263">Or, if you have Storm on your development environment:</span></span>

    ```bash
    storm jar target/WordCount-1.0-SNAPSHOT.jar org.apache.storm.flux.Flux --local /path/to/newtopology.yaml
    ```

    <span data-ttu-id="9c4c0-264">Módosítsa a `/path/to/newtopology.yaml` elérési útját az előző lépésben létrehozott newtopology.yaml fájlt.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-264">Change the `/path/to/newtopology.yaml` to the path to the newtopology.yaml file you created in the previous step.</span></span> <span data-ttu-id="9c4c0-265">Ez a parancs a newtopology.yaml használja, mint a topológia meghatározása.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-265">This command uses the newtopology.yaml as the topology definition.</span></span> <span data-ttu-id="9c4c0-266">Mivel jelenleg nem tartozik a `compile` paraméter, a Maven az előző lépésben létrehozott projekt verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-266">Since we didn't include the `compile` parameter, Maven uses the version of the project built in previous steps.</span></span>

    <span data-ttu-id="9c4c0-267">Ha a topológia elindul, kell figyelje meg, hogy a kibocsátott kötegek között eltelő idő megfelelően newtopology.yaml értéke megváltozott.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-267">Once the topology starts, you should notice that the time between emitted batches has changed to reflect the value in newtopology.yaml.</span></span> <span data-ttu-id="9c4c0-268">Így tudja módosítani a konfigurációs fájl YAM anélkül, hogy le kell fordítani a topológia látható.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-268">So you can see that you can change your configuration through a YAML file without having to recompile the topology.</span></span>

<span data-ttu-id="9c4c0-269">Ezeket és más szolgáltatások fluxus keretrendszer további információkért lásd: [fluxus (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-269">For more information on these and other features of the Flux framework, see [Flux (https://storm.apache.org/releases/0.10.0/flux.html)](https://storm.apache.org/releases/0.10.0/flux.html).</span></span>

## <a name="trident"></a><span data-ttu-id="9c4c0-270">Trident</span><span class="sxs-lookup"><span data-stu-id="9c4c0-270">Trident</span></span>

<span data-ttu-id="9c4c0-271">Trident egy magas szintű absztrakció, Storm által biztosított.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-271">Trident is a high-level abstraction that is provided by Storm.</span></span> <span data-ttu-id="9c4c0-272">Állapot-nyilvántartó feldolgozási támogatja.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-272">It supports stateful processing.</span></span> <span data-ttu-id="9c4c0-273">Az elsődleges Trident előnye, hogy azt is garantálja, hogy a topológia minden üzenetet csak egyszer dolgozza fel.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-273">The primary advantage of Trident is that it can guarantee that every message that enters the topology is processed only once.</span></span> <span data-ttu-id="9c4c0-274">A Trident ezzel szemben nélkül a topológia is csak garantálja, hogy az üzenetek legalább egyszer fel.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-274">Without using Trident, your topology can only guarantee that messages are processed at least once.</span></span> <span data-ttu-id="9c4c0-275">Is más különbségek vannak, például a beépített összetevők boltokhoz létrehozása helyett használható.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-275">There are also other differences, such as built-in components that can be used instead of creating bolts.</span></span> <span data-ttu-id="9c4c0-276">Valójában boltokhoz kevésbé általános összetevők, például a szűrőket, a leképezések és a funkciók helyébe lép.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-276">In fact, bolts are replaced by less-generic components, such as filters, projections, and functions.</span></span>

<span data-ttu-id="9c4c0-277">Trident alkalmazások Maven-projektek használatával is létrehozható.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-277">Trident applications can be created by using Maven projects.</span></span> <span data-ttu-id="9c4c0-278">Az alapvető lépéseket használja, az ebben a cikkben bemutatott – csak a kód nem egyezik.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-278">You use the same basic steps as presented earlier in this article—only the code is different.</span></span> <span data-ttu-id="9c4c0-279">Trident is nem (jelenleg) használható fluxus keretében.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-279">Trident also cannot (currently) be used with the Flux framework.</span></span>

<span data-ttu-id="9c4c0-280">A Trident kapcsolatos további információkért tekintse meg a [Trident API – áttekintés](http://storm.apache.org/documentation/Trident-API-Overview.html).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-280">For more information about Trident, see the [Trident API Overview](http://storm.apache.org/documentation/Trident-API-Overview.html).</span></span>

<span data-ttu-id="9c4c0-281">A Trident alkalmazás példáért lásd: [trendekkel kapcsolatos témakörök a HDInsight alatt futó Apache Storm Twitter](hdinsight-storm-twitter-trending.md).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-281">For an example of a Trident application, see [Twitter trending topics with Apache Storm on HDInsight](hdinsight-storm-twitter-trending.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9c4c0-282">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9c4c0-282">Next Steps</span></span>

<span data-ttu-id="9c4c0-283">Rendelkezik megtudta, hogyan hozhat létre a Storm-topológia Java használatával.</span><span class="sxs-lookup"><span data-stu-id="9c4c0-283">You have learned how to create a Storm topology by using Java.</span></span> <span data-ttu-id="9c4c0-284">Most megtudhatja, hogyan:</span><span class="sxs-lookup"><span data-stu-id="9c4c0-284">Now learn how to:</span></span>

* [<span data-ttu-id="9c4c0-285">Központi telepítése és kezelése a HDInsight alatt futó Apache Storm-topológiák</span><span class="sxs-lookup"><span data-stu-id="9c4c0-285">Deploy and manage Apache Storm topologies on HDInsight</span></span>](hdinsight-storm-deploy-monitor-topology.md)

* [<span data-ttu-id="9c4c0-286">Visual Studio használatával HDInsight alatt futó Apache Storm a C#-topológiák fejlesztése</span><span class="sxs-lookup"><span data-stu-id="9c4c0-286">Develop C# topologies for Apache Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="9c4c0-287">Található további példa Storm-topológiák ellátogatva [a HDInsight alatt futó Storm](hdinsight-storm-example-topology.md).</span><span class="sxs-lookup"><span data-stu-id="9c4c0-287">You can find more example Storm topologies by visiting [Example topologies for Storm on HDInsight](hdinsight-storm-example-topology.md).</span></span>

