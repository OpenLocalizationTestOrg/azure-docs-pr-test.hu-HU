---
title: "A Spark-fürtök - Azure HDInsight futtathatnak Scala-alkalmazás létrehozása |} Microsoft Docs"
description: "Az Apache Maven scalában írt a buildelési rendszer és egy meglévő Maven archetype az IntelliJ IDEA által biztosított Scala Spark-alkalmazás létrehozása."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: b2467a40-a340-4b80-bb00-f2c3339db57b
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: nitinme
ms.openlocfilehash: 95dba08744357f8800b05e3d4b892e3a363d5985
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-scala-maven-application-to-run-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="4f8fc-103">Az Apache Spark-fürttel hdinsighton futó Scala Maven-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f8fc-103">Create a Scala Maven application to run on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="4f8fc-104">Megtudhatja, hogyan hozhat létre az IntelliJ IDEA Maven használatával scalában írt Spark alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-104">Learn how to create a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="4f8fc-105">A cikk Apache Maven build rendszert használ, és az IntelliJ IDEA által biztosított Scala egy meglévő Maven archetype kezdődik.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-105">The article uses Apache Maven as the build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="4f8fc-106">Az IntelliJ IDEA a Scala-alkalmazások létrehozása a következő lépésekből áll:</span><span class="sxs-lookup"><span data-stu-id="4f8fc-106">Creating a Scala application in IntelliJ IDEA involves the following steps:</span></span>

* <span data-ttu-id="4f8fc-107">Maven használata a buildelési rendszer.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-107">Use Maven as the build system.</span></span>
* <span data-ttu-id="4f8fc-108">Frissítse a projekt Object Model (POM) fájlját, hogy a modul Spark-függőségek feloldása.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-108">Update Project Object Model (POM) file to resolve Spark module dependencies.</span></span>
* <span data-ttu-id="4f8fc-109">Írja be az alkalmazás Scala.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-109">Write your application in Scala.</span></span>
* <span data-ttu-id="4f8fc-110">Egy HDInsight Spark-fürtjei küldheti el jar-fájlt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-110">Generate a jar file that can be submitted to HDInsight Spark clusters.</span></span>
* <span data-ttu-id="4f8fc-111">Futtassa az alkalmazást a Spark-fürt Livy használatával.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-111">Run the application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="4f8fc-112">A HDInsight emellett az IntelliJ IDEA beépülő modul eszköz létrehozása és alkalmazások egy HDInsight Spark-fürt Linux rendszeren való elküldése folyamatának megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-112">HDInsight also provides an IntelliJ IDEA plugin tool to ease the process of creating and submitting applications to an HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="4f8fc-113">További információkért lásd: [használata a HDInsight-eszközei beépülő IntelliJ Idea létrehozásához és elküldéséhez a Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA to create and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="4f8fc-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4f8fc-114">Prerequisites</span></span>

* <span data-ttu-id="4f8fc-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-115">An Azure subscription.</span></span> <span data-ttu-id="4f8fc-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="4f8fc-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="4f8fc-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="4f8fc-119">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-119">Oracle Java Development kit.</span></span> <span data-ttu-id="4f8fc-120">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="4f8fc-121">A Java IDE.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-121">A Java IDE.</span></span> <span data-ttu-id="4f8fc-122">Ebben a cikkben az IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="4f8fc-123">A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="4f8fc-124">Az IntelliJ Idea Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="4f8fc-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="4f8fc-125">IntelliJ IDEA telepítési volt nem kéri a felhasználót a Scala beépülő modul engedélyezése, ha indítsa el az IntelliJ IDEA, és hajtsa végre a következő lépésekkel telepíti a beépülő modul:</span><span class="sxs-lookup"><span data-stu-id="4f8fc-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through the following steps to install the plugin:</span></span>

1. <span data-ttu-id="4f8fc-126">Indítsa el az IntelliJ IDEA és az üdvözlőképernyőn kattintson **konfigurálása** majd **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Scala beépülő modul engedélyezése](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="4f8fc-128">A következő képernyőn kattintson **JetBrains telepítése beépülő modul** a bal alsó sarkában.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-128">In the next screen, click **Install JetBrains plugin** from the lower left corner.</span></span> <span data-ttu-id="4f8fc-129">Az a **Tallózás JetBrains beépülő modulok** párbeszédpanel megnyílik, keresse meg a Scala, és kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-129">In the **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Scala beépülő modul telepítése](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="4f8fc-131">A beépülő modul sikeres telepítése után kattintson a **indítsa újra az IntelliJ IDEA gomb** az IDE újraindítására.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-131">After the plugin installs successfully, click the **Restart IntelliJ IDEA button** to restart the IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="4f8fc-132">Önálló Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="4f8fc-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="4f8fc-133">Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="4f8fc-134">Az új projekt párbeszédpanel a következők közül választhat, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-134">In the new project dialog box, make the following choices, and then click **Next**.</span></span>
   
    ![Maven-projekt létrehozása](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="4f8fc-136">Válassza ki **Maven** a projekt típusként.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-136">Select **Maven** as the project type.</span></span>
   * <span data-ttu-id="4f8fc-137">Adjon meg egy **SDK projekt**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="4f8fc-138">Az új gombra, és keresse meg a Java telepítési könyvtárát, általában `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-138">Click New and navigate to the Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="4f8fc-139">Válassza ki a **létrehozása a archetype** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-139">Select the **Create from archetype** option.</span></span>
   * <span data-ttu-id="4f8fc-140">Archetypes listában jelölje ki **org.scala tools.archetypes:scala-archetype-egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-140">From the list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="4f8fc-141">Ez hozza létre a megfelelő könyvtárstruktúrát, és töltse le a szükséges alapértelmezett függőségek Scala program írni.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-141">This will create the right directory structure and download the required default dependencies to write Scala program.</span></span>
2. <span data-ttu-id="4f8fc-142">Adja meg a megfelelő értékeket **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="4f8fc-143">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-143">Click **Next**.</span></span>
3. <span data-ttu-id="4f8fc-144">A következő párbeszédpanelen, amelyben meg kell határoznia Maven kezdőkönyvtár és egyéb felhasználói beállításokat, fogadja el az alapértelmezett beállításokat, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-144">In the next dialog box, where you specify Maven home directory and other user settings, accept the defaults and click **Next**.</span></span>
4. <span data-ttu-id="4f8fc-145">A legutóbbi párbeszédpanelen adja meg a projekt nevét és helyét, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-145">In the last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="4f8fc-146">Törölje a **MySpec.Scala** a(z) **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-146">Delete the **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="4f8fc-147">Nem kell ezt az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-147">You do not need this for the application.</span></span>
6. <span data-ttu-id="4f8fc-148">Ha szükséges, nevezze át az alapértelmezett forrás- és a vizsgált fájlok.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-148">If required, rename the default source and test files.</span></span> <span data-ttu-id="4f8fc-149">Lépjen a bal oldali ablaktáblán az IntelliJ IDEA, **src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-149">From the left pane in the IntelliJ IDEA, navigate to **src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="4f8fc-150">Kattintson a jobb gombbal **App.scala**, kattintson a **Refactor**, kattintson a fájl. Nevezze át, és a párbeszédpanelen adja meg az új nevet az alkalmazásnak, majd **Refactor**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in the dialog box, provide the new name for the application and then click **Refactor**.</span></span>
   
    ![Fájl átnevezése](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="4f8fc-152">A későbbi lépésekben akkor frissíti a pom.xml megadhatók a Spark Scala alkalmazás függőségeit.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-152">In the subsequent steps, you will update the pom.xml to define the dependencies for the Spark Scala application.</span></span> <span data-ttu-id="4f8fc-153">Ezeket a függőségeket letölthető, és automatikusan megoldani ennek megfelelően Maven kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-153">For those dependencies to be downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Automatikus letöltés Maven konfigurálása](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="4f8fc-155">Az a **fájl** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-155">From the **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="4f8fc-156">Az a **beállítások** párbeszédpanel navigáljon a **Build, a végrehajtási, a központi telepítési** > **Build Tools** > **Maven** > **importálása**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-156">In the **Settings** dialog box, navigate to **Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="4f8fc-157">Jelölje be a **Import Maven projektek automatikusan**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-157">Select the option to **Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="4f8fc-158">Kattintson a **alkalmaz**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="4f8fc-159">Frissítse a Scala forrásfájl tartalmazza az alkalmazás kódjában.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-159">Update the Scala source file to include your application code.</span></span> <span data-ttu-id="4f8fc-160">Nyissa meg, és cserélje le a meglévő mintakód a következő kódra, és menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-160">Open and replace the existing sample code with the following code and save the changes.</span></span> <span data-ttu-id="4f8fc-161">Ez a kód beolvassa az adatokat (az összes HDInsight Spark-fürtjei elérhető) HVAC.csv, lekéri a sor csak egy számot a hatodik oszlopban és ír a kimeneti **/HVACOut** alatt az alapértelmezett tároló, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-161">This code reads the data from the HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that only have one digit in the sixth column, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO to wasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find the rows which have only one digit in the 7th column in the CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. <span data-ttu-id="4f8fc-162">Frissítse a pom.xml.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-162">Update the pom.xml.</span></span>
   
   1. <span data-ttu-id="4f8fc-163">Belül `<project>\<properties>` adja hozzá a következő:</span><span class="sxs-lookup"><span data-stu-id="4f8fc-163">Within `<project>\<properties>` add the following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="4f8fc-164">Belül `<project>\<dependencies>` adja hozzá a következő:</span><span class="sxs-lookup"><span data-stu-id="4f8fc-164">Within `<project>\<dependencies>` add the following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="4f8fc-165">Pom.xml módosításainak mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-165">Save changes to pom.xml.</span></span>
10. <span data-ttu-id="4f8fc-166">Hozza létre a .jar fájlt.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-166">Create the .jar file.</span></span> <span data-ttu-id="4f8fc-167">IntelliJ IDEA lehetővé teszi a JAR létrehozását, a projekt összetevő.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="4f8fc-168">A következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-168">Perform the following steps.</span></span>
    
    1. <span data-ttu-id="4f8fc-169">Az a **fájl** menüben kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-169">From the **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="4f8fc-170">Az a **szerkezetének** párbeszédpanel, kattintson a **összetevők** és kattintson a plusz jelre.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-170">In the **Project Structure** dialog box, click **Artifacts** and then click the plus symbol.</span></span> <span data-ttu-id="4f8fc-171">Az előugró párbeszédpanelen kattintson **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-171">From the pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="4f8fc-173">Az a **modulokban létrehozása JAR** párbeszédpanel párbeszédpanelen kattintson a három pont (![három pont](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) szemben a **fő osztály**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-173">In the **Create JAR from Modules** dialog box, click the ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against the **Main Class**.</span></span>
    4. <span data-ttu-id="4f8fc-174">Az a **fő osztály kiválasztása** párbeszédpanel mezőben, válassza ki az osztályt, amely alapértelmezés szerint jelenik meg, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-174">In the **Select Main Class** dialog box, select the class that appears by default and then click **OK**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="4f8fc-176">Az a **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy lehetőség **bontsa ki a cél JAR** van kiválasztva, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-176">In the **Create JAR from Modules** dialog box, make sure that the option to **extract to the target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="4f8fc-177">Ez minden függőség egyetlen JAR hoz létre.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="4f8fc-179">A kimeneti elrendezés lap felsorolja az összes a JAR-fájlok kivételével, amelyek tartalmazzák a Maven project a.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-179">The output layout tab lists all the jars that are included as part of the Maven project.</span></span> <span data-ttu-id="4f8fc-180">Válassza ki, és törölheti azokat, amelyeken a Scala alkalmazásnak nincs közvetlen függőség.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-180">You can select and delete the ones on which the Scala application has no direct dependency.</span></span> <span data-ttu-id="4f8fc-181">Itt létrehozzuk az alkalmazás is távolítja el a legutóbb (**SparkSimpleApp fordítási kimeneti**).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-181">For the application we are creating here, you can remove all but the last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="4f8fc-182">A JAR-fájlok kivételével törli, majd válassza ki a **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-182">Select the jars to delete and then click the **Delete** icon.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="4f8fc-184">Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy a jar minden alkalommal létrejön a projekt beépített vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-184">Make sure **Build on make** box is selected, which ensures that the jar is created every time the project is built or updated.</span></span> <span data-ttu-id="4f8fc-185">Kattintson a **alkalmazása** , majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="4f8fc-186">A menüsávban kattintson **Build**, és kattintson a **ellenőrizze projekt**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-186">From the menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="4f8fc-187">Is **Build összetevők** a jar létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-187">You can also click **Build Artifacts** to create the jar.</span></span> <span data-ttu-id="4f8fc-188">A kimeneti jar alatt létrejön **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-188">The output jar is created under **\out\artifacts**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-the-application-on-the-spark-cluster"></a><span data-ttu-id="4f8fc-190">Az alkalmazás futtatása Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="4f8fc-190">Run the application on the Spark cluster</span></span>
<span data-ttu-id="4f8fc-191">Az alkalmazás futtatásához a fürtön, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="4f8fc-191">To run the application on the cluster, you must do the following:</span></span>

* <span data-ttu-id="4f8fc-192">**Az alkalmazás jar másolása az Azure storage-blob** a fürthöz rendelt.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-192">**Copy the application jar to the Azure storage blob** associated with the cluster.</span></span> <span data-ttu-id="4f8fc-193">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), parancssori segédprogram, ehhez.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, to do so.</span></span> <span data-ttu-id="4f8fc-194">Nincsenek feltölteni az adatokat használó más ügyfelek is számos.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-194">There are a lot of other clients as well that you can use to upload data.</span></span> <span data-ttu-id="4f8fc-195">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="4f8fc-196">**Egy alkalmazás feladat távolról küldhetnek a Livy használatával** a Spark-fürt számára.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-196">**Use Livy to submit an application job remotely** to the Spark cluster.</span></span> <span data-ttu-id="4f8fc-197">A HDInsight Spark-fürtök REST-végpontok távolról a Spark feladatok küldéséhez elérhetővé tévő Livy tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints to remotely submit Spark jobs.</span></span> <span data-ttu-id="4f8fc-198">További információkért lásd: [Livy távolról használata Spark-fürtök a HDInsight Spark küldje el feladatok](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="4f8fc-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="4f8fc-199">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="4f8fc-199">Next step</span></span>

<span data-ttu-id="4f8fc-200">Ebben a cikkben megtanulta, hogyan hozhat létre egy Spark scala alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-200">In this article you learned how to create a Spark scala application.</span></span> <span data-ttu-id="4f8fc-201">Előzetes hogyan futtathatják az alkalmazást egy HDInsight Spark-fürt Livy használatával a következő cikket.</span><span class="sxs-lookup"><span data-stu-id="4f8fc-201">Advance to the next article to learn how to run this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="4f8fc-202">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="4f8fc-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

