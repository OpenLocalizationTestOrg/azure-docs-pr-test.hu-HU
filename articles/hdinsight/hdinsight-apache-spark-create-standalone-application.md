---
title: "aaaCreate Scala app toorun a Spark-fürtjei - Azure HDInsight |} Microsoft Docs"
description: "Az Apache Maven scalában írt, hello rendszer és egy meglévő Maven archetype építése IntelliJ IDEA által biztosított Scala Spark-alkalmazás létrehozása."
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
ms.openlocfilehash: b25291b60921021486f55d78b4832a070a54d163
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-scala-maven-application-toorun-on-apache-spark-cluster-on-hdinsight"></a><span data-ttu-id="2bd03-103">Hozzon létre egy Scala Maven alkalmazás toorun az Apache Spark-fürttel hdinsighton</span><span class="sxs-lookup"><span data-stu-id="2bd03-103">Create a Scala Maven application toorun on Apache Spark cluster on HDInsight</span></span>

<span data-ttu-id="2bd03-104">Megtudhatja, hogyan toocreate a Maven használata IntelliJ IDEA scalában írt Spark-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2bd03-104">Learn how toocreate a Spark application written in Scala using Maven with IntelliJ IDEA.</span></span> <span data-ttu-id="2bd03-105">hello a cikkben az Apache Maven, hello rendszert, és az IntelliJ IDEA által biztosított Scala egy meglévő Maven archetype kezdődik.</span><span class="sxs-lookup"><span data-stu-id="2bd03-105">hello article uses Apache Maven as hello build system and starts with an existing Maven archetype for Scala provided by IntelliJ IDEA.</span></span>  <span data-ttu-id="2bd03-106">A Scala-alkalmazások létrehozása az IntelliJ IDEA magában foglalja a hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2bd03-106">Creating a Scala application in IntelliJ IDEA involves hello following steps:</span></span>

* <span data-ttu-id="2bd03-107">Hello buildelési rendszer Maven használata.</span><span class="sxs-lookup"><span data-stu-id="2bd03-107">Use Maven as hello build system.</span></span>
* <span data-ttu-id="2bd03-108">Projekt Object Model (POM) fájl tooresolve Spark modul függőségek frissítése.</span><span class="sxs-lookup"><span data-stu-id="2bd03-108">Update Project Object Model (POM) file tooresolve Spark module dependencies.</span></span>
* <span data-ttu-id="2bd03-109">Írja be az alkalmazás Scala.</span><span class="sxs-lookup"><span data-stu-id="2bd03-109">Write your application in Scala.</span></span>
* <span data-ttu-id="2bd03-110">A jar-fájlra, amely elküldött tooHDInsight Spark-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2bd03-110">Generate a jar file that can be submitted tooHDInsight Spark clusters.</span></span>
* <span data-ttu-id="2bd03-111">Futtassa a hello alkalmazást a Spark-fürt Livy használatával.</span><span class="sxs-lookup"><span data-stu-id="2bd03-111">Run hello application on Spark cluster using Livy.</span></span>

> [!NOTE]
> <span data-ttu-id="2bd03-112">HDInsight is biztosít az IntelliJ IDEA beépülő modul eszköz tooease hello folyamat létrehozása és elküldése alkalmazások tooan HDInsight Spark-fürt Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2bd03-112">HDInsight also provides an IntelliJ IDEA plugin tool tooease hello process of creating and submitting applications tooan HDInsight Spark cluster on Linux.</span></span> <span data-ttu-id="2bd03-113">További információkért lásd: [használata a HDInsight-eszközei beépülő moduljának IntelliJ IDEA toocreate, és küldje el a Spark-alkalmazások](hdinsight-apache-spark-intellij-tool-plugin.md).</span><span class="sxs-lookup"><span data-stu-id="2bd03-113">For more information, see [Use HDInsight Tools Plugin for IntelliJ IDEA toocreate and submit Spark applications](hdinsight-apache-spark-intellij-tool-plugin.md).</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="2bd03-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2bd03-114">Prerequisites</span></span>

* <span data-ttu-id="2bd03-115">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2bd03-115">An Azure subscription.</span></span> <span data-ttu-id="2bd03-116">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="2bd03-116">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="2bd03-117">A HDInsight az Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="2bd03-117">An Apache Spark cluster on HDInsight.</span></span> <span data-ttu-id="2bd03-118">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="2bd03-118">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
* <span data-ttu-id="2bd03-119">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="2bd03-119">Oracle Java Development kit.</span></span> <span data-ttu-id="2bd03-120">A későbbiekben telepítheti az [Itt](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="2bd03-120">You can install it from [here](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
* <span data-ttu-id="2bd03-121">A Java IDE.</span><span class="sxs-lookup"><span data-stu-id="2bd03-121">A Java IDE.</span></span> <span data-ttu-id="2bd03-122">Ebben a cikkben az IntelliJ IDEA 15.0.1.</span><span class="sxs-lookup"><span data-stu-id="2bd03-122">This article uses IntelliJ IDEA 15.0.1.</span></span> <span data-ttu-id="2bd03-123">A későbbiekben telepítheti az [Itt](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="2bd03-123">You can install it from [here](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-scala-plugin-for-intellij-idea"></a><span data-ttu-id="2bd03-124">Az IntelliJ Idea Scala beépülő modul telepítése</span><span class="sxs-lookup"><span data-stu-id="2bd03-124">Install Scala plugin for IntelliJ IDEA</span></span>
<span data-ttu-id="2bd03-125">IntelliJ IDEA telepítési volt nem kéri a felhasználót a Scala beépülő modul engedélyezése, ha indítsa el az IntelliJ IDEA, és hajtsa végre a következő lépéseket tooinstall hello beépülő modul hello:</span><span class="sxs-lookup"><span data-stu-id="2bd03-125">If IntelliJ IDEA installation did not not prompt for enabling Scala plugin, launch IntelliJ IDEA and go through hello following steps tooinstall hello plugin:</span></span>

1. <span data-ttu-id="2bd03-126">Indítsa el az IntelliJ IDEA és az üdvözlőképernyőn kattintson **konfigurálása** majd **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-126">Start IntelliJ IDEA and from welcome screen click **Configure** and then click **Plugins**.</span></span>
   
    ![Scala beépülő modul engedélyezése](./media/hdinsight-apache-spark-create-standalone-application/enable-scala-plugin.png)
2. <span data-ttu-id="2bd03-128">Hello következő képernyőn kattintson **JetBrains telepítése beépülő modul** a hello bal alsó sarokban.</span><span class="sxs-lookup"><span data-stu-id="2bd03-128">In hello next screen, click **Install JetBrains plugin** from hello lower left corner.</span></span> <span data-ttu-id="2bd03-129">A hello **Tallózás JetBrains beépülő modulok** párbeszédpanel megnyílik, keresse meg a Scala, és kattintson **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-129">In hello **Browse JetBrains Plugins** dialog box that opens, search for Scala and then click **Install**.</span></span>
   
    ![Scala beépülő modul telepítése](./media/hdinsight-apache-spark-create-standalone-application/install-scala-plugin.png)
3. <span data-ttu-id="2bd03-131">Hello beépülő modul sikeres telepítése után kattintson a hello **indítsa újra az IntelliJ IDEA gomb** toorestart hello IDE.</span><span class="sxs-lookup"><span data-stu-id="2bd03-131">After hello plugin installs successfully, click hello **Restart IntelliJ IDEA button** toorestart hello IDE.</span></span>

## <a name="create-a-standalone-scala-project"></a><span data-ttu-id="2bd03-132">Önálló Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="2bd03-132">Create a standalone Scala project</span></span>
1. <span data-ttu-id="2bd03-133">Indítsa el az IntelliJ IDEA, és hozzon létre egy új projektet.</span><span class="sxs-lookup"><span data-stu-id="2bd03-133">Launch IntelliJ IDEA and create a new project.</span></span> <span data-ttu-id="2bd03-134">Hello új projekt párbeszédpanel, győződjön meg a következő lehetőségek hello, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-134">In hello new project dialog box, make hello following choices, and then click **Next**.</span></span>
   
    ![Maven-projekt létrehozása](./media/hdinsight-apache-spark-create-standalone-application/create-maven-project.png)
   
   * <span data-ttu-id="2bd03-136">Válassza ki **Maven** hello projekt típusként.</span><span class="sxs-lookup"><span data-stu-id="2bd03-136">Select **Maven** as hello project type.</span></span>
   * <span data-ttu-id="2bd03-137">Adjon meg egy **SDK projekt**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-137">Specify a **Project SDK**.</span></span> <span data-ttu-id="2bd03-138">Az új gombra, és keresse meg a toohello Java telepítési könyvtárat, általában `C:\Program Files\Java\jdk1.8.0_66`.</span><span class="sxs-lookup"><span data-stu-id="2bd03-138">Click New and navigate toohello Java installation directory, typically `C:\Program Files\Java\jdk1.8.0_66`.</span></span>
   * <span data-ttu-id="2bd03-139">Jelölje be hello **létrehozása a archetype** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2bd03-139">Select hello **Create from archetype** option.</span></span>
   * <span data-ttu-id="2bd03-140">Archetypes hello listában jelölje ki **org.scala tools.archetypes:scala-archetype-egyszerű**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-140">From hello list of archetypes, select **org.scala-tools.archetypes:scala-archetype-simple**.</span></span> <span data-ttu-id="2bd03-141">Ez létrehoz hello jobb könyvtárszerkezetét és hello szükséges alapértelmezett függőségek toowrite Scala program letöltése.</span><span class="sxs-lookup"><span data-stu-id="2bd03-141">This will create hello right directory structure and download hello required default dependencies toowrite Scala program.</span></span>
2. <span data-ttu-id="2bd03-142">Adja meg a megfelelő értékeket **GroupId**, **artifactid szakaszát**, és **verzió**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-142">Provide relevant values for **GroupId**, **ArtifactId**, and **Version**.</span></span> <span data-ttu-id="2bd03-143">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="2bd03-143">Click **Next**.</span></span>
3. <span data-ttu-id="2bd03-144">Hello következő párbeszédpanelen, amelyben meg kell határoznia Maven kezdőkönyvtár és egyéb felhasználói beállításokat, fogadja el a hello alapértelmezett beállításokat, majd kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-144">In hello next dialog box, where you specify Maven home directory and other user settings, accept hello defaults and click **Next**.</span></span>
4. <span data-ttu-id="2bd03-145">A hello utolsó párbeszédpanelen adja meg a projekt nevét és helyét, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-145">In hello last dialog box, specify a project name and location and then click **Finish**.</span></span>
5. <span data-ttu-id="2bd03-146">Törölje a hello **MySpec.Scala** a(z) **src\test\scala\com\microsoft\spark\example**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-146">Delete hello **MySpec.Scala** file at **src\test\scala\com\microsoft\spark\example**.</span></span> <span data-ttu-id="2bd03-147">Nem kell a hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2bd03-147">You do not need this for hello application.</span></span>
6. <span data-ttu-id="2bd03-148">Ha szükséges, nevezze át a hello alapértelmezett forrás- és a vizsgálati fájlokat.</span><span class="sxs-lookup"><span data-stu-id="2bd03-148">If required, rename hello default source and test files.</span></span> <span data-ttu-id="2bd03-149">Az IntelliJ IDEA hello hello bal oldali ablaktáblán, keresse meg túl**src\main\scala\com.microsoft.spark.example**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-149">From hello left pane in hello IntelliJ IDEA, navigate too**src\main\scala\com.microsoft.spark.example**.</span></span> <span data-ttu-id="2bd03-150">Kattintson a jobb gombbal **App.scala**, kattintson a **Refactor**, kattintson a fájl. Nevezze át, és a hello párbeszédpanelen adja meg a hello hello alkalmazás új nevét, majd **Refactor**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-150">Right-click **App.scala**, click **Refactor**, click Rename file, and in hello dialog box, provide hello new name for hello application and then click **Refactor**.</span></span>
   
    ![Fájl átnevezése](./media/hdinsight-apache-spark-create-standalone-application/rename-scala-files.png)  
7. <span data-ttu-id="2bd03-152">A későbbi lépésekben hello frissíteni fogja a Spark Scala alkalmazás hello hello pom.xml toodefine hello függőségeit.</span><span class="sxs-lookup"><span data-stu-id="2bd03-152">In hello subsequent steps, you will update hello pom.xml toodefine hello dependencies for hello Spark Scala application.</span></span> <span data-ttu-id="2bd03-153">Azok a toobe letöltött, és automatikusan feloldani a Maven ennek megfelelően kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="2bd03-153">For those dependencies toobe downloaded and resolved automatically, you must configure Maven accordingly.</span></span>
   
    ![Automatikus letöltés Maven konfigurálása](./media/hdinsight-apache-spark-create-standalone-application/configure-maven.png)
   
   1. <span data-ttu-id="2bd03-155">A hello **fájl** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-155">From hello **File** menu, click **Settings**.</span></span>
   2. <span data-ttu-id="2bd03-156">A hello **beállítások** párbeszédpanelen válassza túl**Build, a végrehajtási, a központi telepítési** > **Build Tools** > **Maven**  >  **Importálása**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-156">In hello **Settings** dialog box, navigate too**Build, Execution, Deployment** > **Build Tools** > **Maven** > **Importing**.</span></span>
   3. <span data-ttu-id="2bd03-157">A beállításnak a hello túl**Import Maven projektek automatikusan**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-157">Select hello option too**Import Maven projects automatically**.</span></span>
   4. <span data-ttu-id="2bd03-158">Kattintson a **alkalmaz**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-158">Click **Apply**, and then click **OK**.</span></span>
8. <span data-ttu-id="2bd03-159">Az alkalmazás kódjában hello Scala forrás fájl tooinclude frissítése</span><span class="sxs-lookup"><span data-stu-id="2bd03-159">Update hello Scala source file tooinclude your application code.</span></span> <span data-ttu-id="2bd03-160">Nyissa meg, és cserélje le a meglévő mintakód a következő kód hello hello és hello módosítások mentése.</span><span class="sxs-lookup"><span data-stu-id="2bd03-160">Open and replace hello existing sample code with hello following code and save hello changes.</span></span> <span data-ttu-id="2bd03-161">Ez a kód hello adatokat olvas hello HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), hello tartalmazó sorok csak egy számjegy hello hatodik oszlopban lekéri és hello kimenetet túl írja**/HVACOut** hello alapértelmezett tároló alatt hello fürt tárolója.</span><span class="sxs-lookup"><span data-stu-id="2bd03-161">This code reads hello data from hello HVAC.csv (available on all HDInsight Spark clusters), retrieves hello rows that only have one digit in hello sixth column, and writes hello output too**/HVACOut** under hello default storage container for hello cluster.</span></span>
   
        package com.microsoft.spark.example
   
        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
   
        /**
          * Test IO toowasb
          */
        object WasbIOTest {
          def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("WASBIOTest")
            val sc = new SparkContext(conf)
   
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
   
            //find hello rows which have only one digit in hello 7th column in hello CSV
            val rdd1 = rdd.filter(s => s.split(",")(6).length() == 1)
   
            rdd1.saveAsTextFile("wasb:///HVACout")
          }
        }
9. <span data-ttu-id="2bd03-162">Hello pom.xml frissítése.</span><span class="sxs-lookup"><span data-stu-id="2bd03-162">Update hello pom.xml.</span></span>
   
   1. <span data-ttu-id="2bd03-163">Belül `<project>\<properties>` adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2bd03-163">Within `<project>\<properties>` add hello following:</span></span>
      
          <scala.version>2.10.4</scala.version>
          <scala.compat.version>2.10.4</scala.compat.version>
          <scala.binary.version>2.10</scala.binary.version>
   2. <span data-ttu-id="2bd03-164">Belül `<project>\<dependencies>` adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2bd03-164">Within `<project>\<dependencies>` add hello following:</span></span>
      
           <dependency>
             <groupId>org.apache.spark</groupId>
             <artifactId>spark-core_${scala.binary.version}</artifactId>
             <version>1.4.1</version>
           </dependency>
      
      <span data-ttu-id="2bd03-165">Mentse a módosításokat toopom.xml.</span><span class="sxs-lookup"><span data-stu-id="2bd03-165">Save changes toopom.xml.</span></span>
10. <span data-ttu-id="2bd03-166">Hozzon létre hello .jar fájlt.</span><span class="sxs-lookup"><span data-stu-id="2bd03-166">Create hello .jar file.</span></span> <span data-ttu-id="2bd03-167">IntelliJ IDEA lehetővé teszi a JAR létrehozását, a projekt összetevő.</span><span class="sxs-lookup"><span data-stu-id="2bd03-167">IntelliJ IDEA enables creation of JAR as an artifact of a project.</span></span> <span data-ttu-id="2bd03-168">Hajtsa végre a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="2bd03-168">Perform hello following steps.</span></span>
    
    1. <span data-ttu-id="2bd03-169">A hello **fájl** menüben kattintson a **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-169">From hello **File** menu, click **Project Structure**.</span></span>
    2. <span data-ttu-id="2bd03-170">A hello **szerkezetének** párbeszédpanel, kattintson a **összetevők** , majd a hello plusz jelre.</span><span class="sxs-lookup"><span data-stu-id="2bd03-170">In hello **Project Structure** dialog box, click **Artifacts** and then click hello plus symbol.</span></span> <span data-ttu-id="2bd03-171">A hello előugró párbeszédpanelen kattintson az **JAR**, és kattintson a **a függőségekkel rendelkező modulok**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-171">From hello pop-up dialog box, click **JAR**, and then click **From modules with dependencies**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-1.png)
    3. <span data-ttu-id="2bd03-173">A hello **modulokban létrehozása JAR** párbeszédpanel kattintson hello három pont (![három pont](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) hello elleni **fő osztály**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-173">In hello **Create JAR from Modules** dialog box, click hello ellipsis (![ellipsis](./media/hdinsight-apache-spark-create-standalone-application/ellipsis.png) ) against hello **Main Class**.</span></span>
    4. <span data-ttu-id="2bd03-174">A hello **fő osztály kiválasztása** párbeszédpanel megnyitásához, jelölje be hello osztály, amely alapértelmezés szerint jelenik meg, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-174">In hello **Select Main Class** dialog box, select hello class that appears by default and then click **OK**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-2.png)
    5. <span data-ttu-id="2bd03-176">A hello **modulokban létrehozása JAR** párbeszédpanelen győződjön meg arról, hogy hello beállítás túl**toohello cél JAR kibontása** van kiválasztva, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-176">In hello **Create JAR from Modules** dialog box, make sure that hello option too**extract toohello target JAR** is selected, and then click **OK**.</span></span> <span data-ttu-id="2bd03-177">Ez minden függőség egyetlen JAR hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2bd03-177">This creates a single JAR with all dependencies.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/create-jar-3.png)
    6. <span data-ttu-id="2bd03-179">hello kimeneti elrendezés lap felsorolja az összes hello JAR-fájlok kivételével, amelyek tartalmazzák a hello Maven project.</span><span class="sxs-lookup"><span data-stu-id="2bd03-179">hello output layout tab lists all hello jars that are included as part of hello Maven project.</span></span> <span data-ttu-id="2bd03-180">Kiválaszthatja, és azokat, amelyeken hello Scala alkalmazás törlése hello nincs közvetlen függőség van.</span><span class="sxs-lookup"><span data-stu-id="2bd03-180">You can select and delete hello ones on which hello Scala application has no direct dependency.</span></span> <span data-ttu-id="2bd03-181">Itt létrehozzuk hello alkalmazáshoz, akkor eltávolíthatja az összes, de az utolsó hello (**SparkSimpleApp fordítási kimeneti**).</span><span class="sxs-lookup"><span data-stu-id="2bd03-181">For hello application we are creating here, you can remove all but hello last one (**SparkSimpleApp compile output**).</span></span> <span data-ttu-id="2bd03-182">Hello JAR-fájlok kivételével toodelete válasszon, majd kattintson a hello **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2bd03-182">Select hello jars toodelete and then click hello **Delete** icon.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/delete-output-jars.png)
       
        <span data-ttu-id="2bd03-184">Győződjön meg arról, hogy **győződjön Build** be van jelölve, amely biztosítja, hogy hello jar minden alkalommal létrejön hello projekt beépített vagy frissíteni.</span><span class="sxs-lookup"><span data-stu-id="2bd03-184">Make sure **Build on make** box is selected, which ensures that hello jar is created every time hello project is built or updated.</span></span> <span data-ttu-id="2bd03-185">Kattintson a **alkalmazása** , majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-185">Click **Apply** and then **OK**.</span></span>
    7. <span data-ttu-id="2bd03-186">Hello menüsorában kattintson **Build**, és kattintson a **ellenőrizze projekt**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-186">From hello menu bar, click **Build**, and then click **Make Project**.</span></span> <span data-ttu-id="2bd03-187">Is **Build összetevők** toocreate hello jar.</span><span class="sxs-lookup"><span data-stu-id="2bd03-187">You can also click **Build Artifacts** toocreate hello jar.</span></span> <span data-ttu-id="2bd03-188">hello kimeneti jar alatt létrejön **\out\artifacts**.</span><span class="sxs-lookup"><span data-stu-id="2bd03-188">hello output jar is created under **\out\artifacts**.</span></span>
       
        ![Hozzon létre JAR](./media/hdinsight-apache-spark-create-standalone-application/output.png)

## <a name="run-hello-application-on-hello-spark-cluster"></a><span data-ttu-id="2bd03-190">Hello Spark-fürt hello az alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="2bd03-190">Run hello application on hello Spark cluster</span></span>
<span data-ttu-id="2bd03-191">toorun hello alkalmazás hello fürtön, el kell végeznie a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2bd03-191">toorun hello application on hello cluster, you must do hello following:</span></span>

* <span data-ttu-id="2bd03-192">**Másolás hello alkalmazás jar toohello Azure storage-blob** hello-fürthöz tartozó.</span><span class="sxs-lookup"><span data-stu-id="2bd03-192">**Copy hello application jar toohello Azure storage blob** associated with hello cluster.</span></span> <span data-ttu-id="2bd03-193">Használhat [ **AzCopy**](../storage/common/storage-use-azcopy.md), a parancs így sor segédprogram, toodo.</span><span class="sxs-lookup"><span data-stu-id="2bd03-193">You can use [**AzCopy**](../storage/common/storage-use-azcopy.md), a command line utility, toodo so.</span></span> <span data-ttu-id="2bd03-194">Nincsenek más ügyfelek is használható tooupload adatok számos.</span><span class="sxs-lookup"><span data-stu-id="2bd03-194">There are a lot of other clients as well that you can use tooupload data.</span></span> <span data-ttu-id="2bd03-195">A rájuk vonatkozó további található [feltölteni az adatokat a HDInsight Hadoop-feladatok](hdinsight-upload-data.md).</span><span class="sxs-lookup"><span data-stu-id="2bd03-195">You can find more about them at [Upload data for Hadoop jobs in HDInsight](hdinsight-upload-data.md).</span></span>
* <span data-ttu-id="2bd03-196">**Egy alkalmazás feladat Livy toosubmit távolról használható** toohello Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="2bd03-196">**Use Livy toosubmit an application job remotely** toohello Spark cluster.</span></span> <span data-ttu-id="2bd03-197">A HDInsight Spark-fürtök REST végpontok tooremotely submit Spark feladatok elérhetővé tévő Livy tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2bd03-197">Spark clusters on HDInsight includes Livy that exposes REST endpoints tooremotely submit Spark jobs.</span></span> <span data-ttu-id="2bd03-198">További információkért lásd: [Livy távolról használata Spark-fürtök a HDInsight Spark küldje el feladatok](hdinsight-apache-spark-livy-rest-interface.md).</span><span class="sxs-lookup"><span data-stu-id="2bd03-198">For more information, see [Submit Spark jobs remotely using Livy with Spark clusters on HDInsight](hdinsight-apache-spark-livy-rest-interface.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="2bd03-199">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="2bd03-199">Next step</span></span>

<span data-ttu-id="2bd03-200">Az e cikkben megtanulta, hogyan meg toocreate a Spark scala-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2bd03-200">In this article you learned how toocreate a Spark scala application.</span></span> <span data-ttu-id="2bd03-201">Előzetes toohello tovább cikk toolearn hogyan toorun az alkalmazást egy HDInsight Spark fürt Livy használatával.</span><span class="sxs-lookup"><span data-stu-id="2bd03-201">Advance toohello next article toolearn how toorun this application on an HDInsight Spark cluster using Livy.</span></span>

> [!div class="nextstepaction"]
>[<span data-ttu-id="2bd03-202">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="2bd03-202">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

