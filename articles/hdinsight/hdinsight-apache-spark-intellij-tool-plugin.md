---
title: "Az IntelliJ Azure eszköztára: Spark-alkalmazások a HDInsight-fürtök létrehozása |} Microsoft Docs"
description: "Az IntelliJ Azure eszköztára használata Spark scalában írt alkalmazások fejlesztéséhez, és egy HDInsight Spark-fürt küldheti el ezeket."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 73304272-6c8b-482e-af7c-cd25d95dab4d
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: nitinme
ms.openlocfilehash: 19cb8f436fa4d86f323013a5d4b3b50bf6c80a1a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-azure-toolkit-for-intellij-to-create-spark-applications-for-an-hdinsight-cluster"></a><span data-ttu-id="1c3a5-103">Az IntelliJ Azure eszköztára használata Spark-alkalmazások a HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-103">Use Azure Toolkit for IntelliJ to create Spark applications for an HDInsight cluster</span></span>

<span data-ttu-id="1c3a5-104">Az Azure-eszközkészlet használata az IntelliJ beépülő modul scalában írt Spark-alkalmazások fejlesztéséhez és majd küldheti el ezeket a HDInsight Spark-fürt közvetlenül a az IntelliJ integrált fejlesztési környezeti (IDE).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-104">Use the Azure Toolkit for IntelliJ plug-in to develop Spark applications written in Scala, and then submit them to an HDInsight Spark cluster directly from the IntelliJ integrated development environment (IDE).</span></span> <span data-ttu-id="1c3a5-105">Használhatja a beépülő modul néhány módon:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-105">You can use the plug-in in a few ways:</span></span>

* <span data-ttu-id="1c3a5-106">Fejleszthet, és küldje el a Scala Spark alkalmazás egy HDInsight Spark-fürtön.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-106">Develop and submit a Scala Spark application on an HDInsight Spark cluster.</span></span>
* <span data-ttu-id="1c3a5-107">Az Azure HDInsight Spark-fürt erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-107">Access your Azure HDInsight Spark cluster resources.</span></span>
* <span data-ttu-id="1c3a5-108">Fejleszthet, és egy Scala Spark alkalmazás helyileg történő futtatása.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-108">Develop and run a Scala Spark application locally.</span></span>

<span data-ttu-id="1c3a5-109">A projekt létrehozásához tekintse meg a [Spark-alkalmazások létrehozása az intellij-t Azure eszközkészlete](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) videó.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-109">To create your project, view the [Create Spark Applications with the Azure Toolkit for IntelliJ](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ) video.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1c3a5-110">Segítségével ez a beépülő modul létrehozása, és küldje el az alkalmazásokat csak egy HDInsight Spark-fürt Linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-110">You can use this plug-in to create and submit applications only for an HDInsight Spark cluster on Linux.</span></span>
> 

## <a name="prerequisites"></a><span data-ttu-id="1c3a5-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1c3a5-111">Prerequisites</span></span>

- <span data-ttu-id="1c3a5-112">HDInsight Linux rendszeren Apache Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-112">An Apache Spark cluster on HDInsight Linux.</span></span> <span data-ttu-id="1c3a5-113">Útmutatásért lásd: [létrehozása az Apache Spark on Azure hdinsight clusters](hdinsight-apache-spark-jupyter-spark-sql.md).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-113">For instructions, see [Create Apache Spark clusters in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).</span></span>
- <span data-ttu-id="1c3a5-114">Oracle Java fejlesztői készlet.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-114">Oracle Java Development Kit.</span></span> <span data-ttu-id="1c3a5-115">Telepítheti azt a [Oracle webhely](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-115">You can install it from the [Oracle website](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html).</span></span>
- <span data-ttu-id="1c3a5-116">IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-116">IntelliJ IDEA.</span></span> <span data-ttu-id="1c3a5-117">Ez a cikk 2017.1 verzióját használja.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-117">This article uses version 2017.1.</span></span> <span data-ttu-id="1c3a5-118">Telepítheti azt a [JetBrains webhely](https://www.jetbrains.com/idea/download/).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-118">You can install it from the [JetBrains website](https://www.jetbrains.com/idea/download/).</span></span>

## <a name="install-azure-toolkit-for-intellij"></a><span data-ttu-id="1c3a5-119">Az intellij-t Azure eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="1c3a5-119">Install Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="1c3a5-120">A telepítési utasításokért lásd: [Azure eszközkészlet telepítése az IntelliJ](../azure-toolkit-for-intellij-installation.md).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-120">For installation instructions, see [Install Azure Toolkit for IntelliJ](../azure-toolkit-for-intellij-installation.md).</span></span>

## <a name="sign-in-to-your-azure-subscription"></a><span data-ttu-id="1c3a5-121">Jelentkezzen be az Azure-előfizetésébe</span><span class="sxs-lookup"><span data-stu-id="1c3a5-121">Sign in to your Azure subscription</span></span>

1. <span data-ttu-id="1c3a5-122">Indítsa el az IntelliJ IDE, és nyissa meg az Azure-kezelővel.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-122">Start the IntelliJ IDE, and open Azure Explorer.</span></span> <span data-ttu-id="1c3a5-123">Az a **nézet** menüjében válassza **eszköz Windows**, majd válassza ki **Azure Explorer**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-123">On the **View** menu, select **Tool Windows**, and then select **Azure Explorer**.</span></span>
       
   ![Az Azure-kezelővel hivatkozás](./media/hdinsight-apache-spark-intellij-tool-plugin/show-azure-explorer.png)

2. <span data-ttu-id="1c3a5-125">Kattintson a jobb gombbal a **Azure** csomópont, és válassza **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-125">Right-click the **Azure** node, and then select **Sign In**.</span></span>

3. <span data-ttu-id="1c3a5-126">Az a **Azure bejelentkezés** párbeszédpanelen jelölje ki **bejelentkezés**, és írja be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-126">In the **Azure Sign In** dialog box, select **Sign in**, and then enter your Azure credentials.</span></span>

    ![Az Azure bejelentkezés párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-2.png)

4. <span data-ttu-id="1c3a5-128">Miután bejelentkezett a, a **előfizetések kiválasztása** párbeszédpanel megjeleníti az összes Azure-előfizetések társított hitelesítő adatok.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-128">After you're signed in, the **Select Subscriptions** dialog box lists all the Azure subscriptions that are associated with the credentials.</span></span> <span data-ttu-id="1c3a5-129">Válassza ki a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-129">Select the **Select** button.</span></span>

    ![Az előfizetések kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Select-Subscriptions.png)

5. <span data-ttu-id="1c3a5-131">Az a **Azure Explorer** lapján bontsa ki a **HDInsight** az előfizetés a HDInsight Spark-fürtjei megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-131">On the **Azure Explorer** tab, expand **HDInsight** to view the HDInsight Spark clusters that are in your subscription.</span></span>
   
    ![HDInsight Spark-fürtjei az Azure-kezelővel](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-3.png)

6. <span data-ttu-id="1c3a5-133">A fürt kapcsolódó erőforrások (például storage-fiókok) megtekintéséhez ennél jobban is kibonthatja a fürtnév csomópont.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-133">To view the resources (for example, storage accounts) that are associated with the cluster, you can further expand a cluster-name node.</span></span>
   
    ![Egy bővített fürtnév csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/view-explorer-4.png)

## <a name="run-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="1c3a5-135">Futtassa a Spark Scala-alkalmazások HDInsight Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="1c3a5-135">Run a Spark Scala application on an HDInsight Spark cluster</span></span>

1. <span data-ttu-id="1c3a5-136">Indítsa el az IntelliJ IDEA, és ezután a projekt létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-136">Start IntelliJ IDEA, and then create a project.</span></span> <span data-ttu-id="1c3a5-137">Az a **új projekt** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-137">In the **New Project** dialog box, do the following:</span></span> 

   <span data-ttu-id="1c3a5-138">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-138">a.</span></span> <span data-ttu-id="1c3a5-139">Válassza ki **HDInsight** > **a Spark on HDInsight (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-139">Select **HDInsight** > **Spark on HDInsight (Scala)**.</span></span>

   <span data-ttu-id="1c3a5-140">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-140">b.</span></span> <span data-ttu-id="1c3a5-141">Az a **Build eszköz** listára, válassza ki, az igényeknek megfelelően az alábbiak valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-141">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="1c3a5-142">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="1c3a5-142">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="1c3a5-143">**SBT**, a függőségek kezelésére, és a Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-143">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Az új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/create-hdi-scala-app.png)

2. <span data-ttu-id="1c3a5-145">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-145">Select **Next**.</span></span>

3. <span data-ttu-id="1c3a5-146">A Scala-projekt létrehozása varázsló automatikusan észleli, hogy telepítette-e a Scala beépülő modult.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-146">The Scala project-creation wizard automatically detects whether you've installed the Scala plug-in.</span></span> <span data-ttu-id="1c3a5-147">Válassza ki **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-147">Select **Install**.</span></span>

   ![Scala beépülő modul ellenőrzése](./media/hdinsight-apache-spark-intellij-tool-plugin/Scala-Plugin-check-Reminder.PNG) 

4. <span data-ttu-id="1c3a5-149">A beépülő modul Scala letöltéséhez, jelölje be az **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-149">To download the Scala plug-in, select **OK**.</span></span> <span data-ttu-id="1c3a5-150">Az utasítások az IntelliJ indítsa újra.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-150">Follow the instructions to restart IntelliJ.</span></span> 

   ![A Scala beépülő modul telepítése párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Choose-Scala-Plugin.PNG)

5. <span data-ttu-id="1c3a5-152">Az a **új projekt** ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-152">In the **New Project** window, do the following:</span></span>  

    ![A Spark SDK kiválasztása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-new-project.png)

   <span data-ttu-id="1c3a5-154">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-154">a.</span></span> <span data-ttu-id="1c3a5-155">Adja meg a projekt nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-155">Enter a project name and location.</span></span>

   <span data-ttu-id="1c3a5-156">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-156">b.</span></span> <span data-ttu-id="1c3a5-157">Az a **projekt SDK** legördülő listában válassza **Java 1.8** a Spark-fürt 2.x, vagy válassza ki a **Java 1.7** a Spark 1.x fürthöz.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-157">In the **Project SDK** drop-down list, select **Java 1.8** for the Spark 2.x cluster, or select **Java 1.7** for the Spark 1.x cluster.</span></span>

   <span data-ttu-id="1c3a5-158">c.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-158">c.</span></span> <span data-ttu-id="1c3a5-159">Az a **Spark verzió** legördülő listából válassza ki, Scala-projekt létrehozása varázsló Spark SDK és Scala SDK integrálja a megfelelő verzióját.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-159">In the **Spark version** drop-down list, Scala project creation wizard integrates the proper version for Spark SDK and Scala SDK.</span></span> <span data-ttu-id="1c3a5-160">Ha a Spark-fürt verziója korábbi, mint 2,0, válassza ki a **Spark 1.x**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-160">If the Spark cluster version is earlier than 2.0, select **Spark 1.x**.</span></span> <span data-ttu-id="1c3a5-161">Máskülönben válassza **Spark2.x**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-161">Otherwise, select **Spark2.x**.</span></span> <span data-ttu-id="1c3a5-162">Ez a példa **Spark 2.0.2 (Scala 2.11.8)**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-162">This example uses **Spark 2.0.2 (Scala 2.11.8)**.</span></span>

6. <span data-ttu-id="1c3a5-163">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-163">Select **Finish**.</span></span>

7. <span data-ttu-id="1c3a5-164">A Spark-projekt automatikusan létrehoz egy összetevő.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-164">The Spark project automatically creates an artifact for you.</span></span> <span data-ttu-id="1c3a5-165">Az összetevő megtekintéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-165">To view the artifact, do the following:</span></span>

   <span data-ttu-id="1c3a5-166">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-166">a.</span></span> <span data-ttu-id="1c3a5-167">Az a **fájl** menü **szerkezetének**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-167">On the **File** menu, select **Project Structure**.</span></span>

   <span data-ttu-id="1c3a5-168">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-168">b.</span></span> <span data-ttu-id="1c3a5-169">Az a **szerkezetének** párbeszédpanelen jelölje ki **összetevők** létrehozott alapértelmezett összetevő megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-169">In the **Project Structure** dialog box, select **Artifacts** to view the default artifact that is created.</span></span> <span data-ttu-id="1c3a5-170">A saját összetevő is létrehozhat; ehhez válassza a plusz jelre (**+**).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-170">You can also create your own artifact by selecting the plus sign (**+**).</span></span>

      ![Összetevő információ párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/default-artifact.png)
      
8. <span data-ttu-id="1c3a5-172">Adja hozzá az alkalmazás forráskódjához, az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-172">Add your application source code by doing the following:</span></span>

   <span data-ttu-id="1c3a5-173">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-173">a.</span></span> <span data-ttu-id="1c3a5-174">A Project Explorer, kattintson a jobb gombbal **src**, mutasson a **új**, majd válassza ki **Scala osztály**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-174">In Project Explorer, right-click **src**, point to **New**, and then select **Scala Class**.</span></span>
      
      ![Parancsok a Project Explorer Scala osztály létrehozása](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code.png)

   <span data-ttu-id="1c3a5-176">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-176">b.</span></span> <span data-ttu-id="1c3a5-177">Az a **hozzon létre új Scala osztály** párbeszédpanel mezőben adjon meg egy nevet, válasszon **objektum** a a **jellegű** mezőbe, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-177">In the **Create New Scala Class** dialog box, provide a name, select **Object** in the **Kind** box, and then select **OK**.</span></span>
      
      ![Hozzon létre új Scala osztály párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-scala-code-object.png)

   <span data-ttu-id="1c3a5-179">c.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-179">c.</span></span> <span data-ttu-id="1c3a5-180">Az a **MyClusterApp.scala** fájlt, az alábbi kódot.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-180">In the **MyClusterApp.scala** file, paste the following code.</span></span> <span data-ttu-id="1c3a5-181">A kód beolvassa az adatokat HVAC.csv (az összes HDInsight Spark-fürtjei elérhető), lekéri a sorokat, csak egy számot a CSV-fájlt az hetedik oszlop és ír a kimeneti **/HVACOut** alatt az alapértelmezett tároló, a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-181">The code reads the data from HVAC.csv (available on all HDInsight Spark clusters), retrieves the rows that have only one digit in the seventh column in the CSV file, and writes the output to **/HVACOut** under the default storage container for the cluster.</span></span>

        import org.apache.spark.SparkConf
        import org.apache.spark.SparkContext
    
        object MyClusterApp{
            def main (arg: Array[String]): Unit = {
            val conf = new SparkConf().setAppName("MyClusterApp")
            val sc = new SparkContext(conf)
    
            val rdd = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
            //find the rows that have only one digit in the seventh column in the CSV file
            val rdd1 =  rdd.filter(s => s.split(",")(6).length() == 1)
    
            rdd1.saveAsTextFile("wasb:///HVACOut")
            }
    
        }

9. <span data-ttu-id="1c3a5-182">Futtassa az alkalmazást egy HDInsight Spark-fürtön a következő módon:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-182">Run the application on an HDInsight Spark cluster by doing the following:</span></span>

   <span data-ttu-id="1c3a5-183">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-183">a.</span></span> <span data-ttu-id="1c3a5-184">A Project Explorer, kattintson a jobb gombbal a projekt nevét, és válassza **küldje el a Spark-alkalmazást, amely**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-184">In Project Explorer, right-click the project name, and then select **Submit Spark Application to HDInsight**.</span></span>
      
      ![A HDInsight parancs küldje el a Spark alkalmazás](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-1.png)

   <span data-ttu-id="1c3a5-186">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-186">b.</span></span> <span data-ttu-id="1c3a5-187">Azure-előfizetés hitelesítő adatait kéri.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-187">You are prompted to enter your Azure subscription credentials.</span></span> <span data-ttu-id="1c3a5-188">Az a **Spark küldésének** párbeszédpanelen adja meg a következő értékeket, és válassza **Submit**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-188">In the **Spark Submission** dialog box, provide the following values, and then select **Submit**.</span></span>
      
      * <span data-ttu-id="1c3a5-189">A **Spark-fürtök (csak Linux)**, válassza ki a HDInsight Spark-fürt, amelyen szeretné futtatni az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-189">For **Spark clusters (Linux only)**, select the HDInsight Spark cluster on which you want to run your application.</span></span>

      * <span data-ttu-id="1c3a5-190">Az IntelliJ projekt összetevő válasszon, vagy válasszon ki egy, a merevlemez-meghajtóról.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-190">Select an artifact from the IntelliJ project, or select one from the hard drive.</span></span>

      * <span data-ttu-id="1c3a5-191">Az a **fő osztálynév** jelölje ki a három pont (**...** ), a fő osztályban található az alkalmazás forráskódjához, majd válassza ki és **OK**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-191">In the **Main class name** box, select the ellipsis (**...**), select the main class in your application source code, and then select **OK**.</span></span>

        ![A fő osztály kiválasztása párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-3.png)

      * <span data-ttu-id="1c3a5-193">Mivel ebben a példában az alkalmazás kódja nem igényel parancssori argumentumot vagy hivatkozás JAR-fájlok kivételével vagy fájlokat, hagyhatja, a többi mező üres.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-193">Because the application code in this example does not require command-line arguments or reference JARs or files, you can leave the remaining boxes empty.</span></span> <span data-ttu-id="1c3a5-194">Miután megadta az adatokat, a párbeszédpanelen az alábbi képen kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-194">After you provide all the information, the dialog box should resemble the following image.</span></span>
        
        ![A Spark küldésének párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-submit-spark-app-2.png)

   <span data-ttu-id="1c3a5-196">c.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-196">c.</span></span> <span data-ttu-id="1c3a5-197">A **Spark küldésének** fülre az ablak alján kell kezdenie a folyamatban lévő megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-197">The **Spark Submission** tab at the bottom of the window should start displaying the progress.</span></span> <span data-ttu-id="1c3a5-198">Az alkalmazás a piros gombra kattintva is leállíthatja a **Spark küldésének** ablak.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-198">You can also stop the application by selecting the red button in the **Spark Submission** window.</span></span>
      
      ![A Spark küldésének ablak](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-result.png)
      
      <span data-ttu-id="1c3a5-200">A feladat kimenetére elérésére, lásd: a "hozzáférés és a HDInsight Spark-fürtök kezelése az intellij-t Azure eszközkészlet használatával" című szakaszban ebben a cikkben található.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-200">To learn how to access the job output, see the "Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ" section later in this article.</span></span>

## <a name="run-or-debug-a-spark-scala-application-on-an-hdinsight-spark-cluster"></a><span data-ttu-id="1c3a5-201">Futtassa, vagy egy HDInsight Spark-fürt Spark Scala alkalmazások hibakeresése</span><span class="sxs-lookup"><span data-stu-id="1c3a5-201">Run or debug a Spark Scala application on an HDInsight Spark cluster</span></span>
<span data-ttu-id="1c3a5-202">Javasoljuk továbbá egy másik módszer a Spark alkalmazás fürtre elküldése.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-202">We also recommend another way of submitting the Spark application to the cluster.</span></span> <span data-ttu-id="1c3a5-203">Ehhez állítsa be a paraméterek a **Futtatás/Debug konfigurációk** IDE.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-203">You can do so by setting the parameters in the **Run/Debug configurations** IDE.</span></span> <span data-ttu-id="1c3a5-204">További információkért lásd: [távolról az IntelliJ SSH-n keresztül a Azure eszközkészlet a HDInsight-fürtök a Spark-alkalmazások hibakeresését](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-204">For more information, see [Remotely debug Spark applications on an HDInsight cluster with Azure Toolkit for IntelliJ through SSH](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh).</span></span>

## <a name="access-and-manage-hdinsight-spark-clusters-by-using-azure-toolkit-for-intellij"></a><span data-ttu-id="1c3a5-205">Férhessen hozzá és felügyelhesse a HDInsight Spark-fürtjei IntelliJ Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="1c3a5-205">Access and manage HDInsight Spark clusters by using Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="1c3a5-206">Az intellij-t Azure eszközkészlet használatával különféle műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-206">You can perform various operations by using Azure Toolkit for IntelliJ.</span></span>

### <a name="access-the-job-view"></a><span data-ttu-id="1c3a5-207">Hozzáférés a feladat megtekintése</span><span class="sxs-lookup"><span data-stu-id="1c3a5-207">Access the job view</span></span>
1. <span data-ttu-id="1c3a5-208">Az Azure Explorerben bontsa ki a **HDInsight**, bontsa ki a Spark-fürt nevét, majd válassza ki **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-208">In Azure Explorer, expand **HDInsight**, expand the Spark cluster name, and then select **Jobs**.</span></span>  

    ![Feladatok nézet csomópont](./media/hdinsight-apache-spark-intellij-tool-plugin/job-view-node.png)

2. <span data-ttu-id="1c3a5-210">A jobb oldali ablaktáblában a **Spark feladat megtekintése** lap megjeleníti a fürtön futó összes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-210">In the right pane, the **Spark Job View** tab displays all the applications that were run on the cluster.</span></span> <span data-ttu-id="1c3a5-211">Válassza ki, amelynek meg szeretné tekinteni a további részleteket az alkalmazás nevét.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-211">Select the name of the application for which you want to see more details.</span></span>

    ![Az alkalmazás részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/view-job-logs.png)

3. <span data-ttu-id="1c3a5-213">Alapszintű futó feladat adatainak megjelenítéséhez vigye a feladat ábra.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-213">To display basic running job information, hover over the job graph.</span></span> <span data-ttu-id="1c3a5-214">A szakaszok grafikon és információt, amely minden feladatot hoz létre megtekintéséhez válasszon ki egy csomópontot a feladat ábra a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-214">To view the stages graph and information that every job generates, select a node on the job graph.</span></span>

    ![Feladat szakasz részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-graph-stage-info.png)

4. <span data-ttu-id="1c3a5-216">Megtekintheti a gyakran használt naplókat, például a *illesztőprogram Stderr*, *illesztőprogram Stdout*, és *Directory Info*, jelölje be a **napló** fülre.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-216">To view frequently used logs, such as *Driver Stderr*, *Driver Stdout*, and *Directory Info*, select the **Log** tab.</span></span>

    ![Naplófájl részletei](./media/hdinsight-apache-spark-intellij-tool-plugin/Job-log-info.png)

5. <span data-ttu-id="1c3a5-218">A Spark-előzmények felhasználói felület és a YARN felhasználói felületen (az alkalmazás szintjén) hivatkozás az ablak tetején kiválasztásával is megtekintheti.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-218">You can also view the Spark history UI and the YARN UI (at the application level) by selecting a link at the top of the window.</span></span>

### <a name="access-the-spark-history-server"></a><span data-ttu-id="1c3a5-219">A Spark-előzmények kiszolgáló elérhető</span><span class="sxs-lookup"><span data-stu-id="1c3a5-219">Access the Spark history server</span></span>
1. <span data-ttu-id="1c3a5-220">Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyissa meg a Spark feladatelőzmények felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-220">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Spark History UI**.</span></span> 

2. <span data-ttu-id="1c3a5-221">Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adataival, amely a fürt telepítésekor megadott.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-221">When you're prompted, enter the cluster's admin credentials, which you specified when you set up the cluster.</span></span>

3. <span data-ttu-id="1c3a5-222">A Spark előzmények server irányítópult az alkalmazás neve segítségével keresse meg az alkalmazás csak futása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-222">On the Spark history server dashboard, you can use the application name to look for the application that you just finished running.</span></span> <span data-ttu-id="1c3a5-223">Az előző kódban használatával beállíthatja az alkalmazás neve `val conf = new SparkConf().setAppName("MyClusterApp")`.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-223">In the preceding code, you set the application name by using `val conf = new SparkConf().setAppName("MyClusterApp")`.</span></span> <span data-ttu-id="1c3a5-224">A Spark alkalmazásnév ezért **MyClusterApp**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-224">Therefore, your Spark application name is **MyClusterApp**.</span></span>

### <a name="start-the-ambari-portal"></a><span data-ttu-id="1c3a5-225">Indítsa el az Ambari portálon</span><span class="sxs-lookup"><span data-stu-id="1c3a5-225">Start the Ambari portal</span></span>
1. <span data-ttu-id="1c3a5-226">Az Azure Explorerben bontsa ki a **HDInsight**, kattintson a jobb gombbal a Spark-fürt nevét, majd válassza ki **nyitott fürt Management Portal (Ambari)**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-226">In Azure Explorer, expand **HDInsight**, right-click your Spark cluster name, and then select **Open Cluster Management Portal (Ambari)**.</span></span> 

2. <span data-ttu-id="1c3a5-227">Amikor a rendszer kéri, adja meg a fürt rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-227">When you're prompted, enter the admin credentials for the cluster.</span></span> <span data-ttu-id="1c3a5-228">Ezeket a hitelesítő adatokat adott meg a fürt telepítés során.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-228">You specified these credentials during the cluster setup process.</span></span>

### <a name="manage-azure-subscriptions"></a><span data-ttu-id="1c3a5-229">Az Azure-előfizetések kezelése</span><span class="sxs-lookup"><span data-stu-id="1c3a5-229">Manage Azure subscriptions</span></span>
<span data-ttu-id="1c3a5-230">Alapértelmezés szerint az intellij-t Azure eszköztára a Spark-fürtök az összes Azure-előfizetések sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-230">By default, Azure Toolkit for IntelliJ lists the Spark clusters from all your Azure subscriptions.</span></span> <span data-ttu-id="1c3a5-231">Ha szükséges, megadhat olyan előfizetést, amely az elérni kívánt.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-231">If necessary, you can specify the subscriptions that you want to access.</span></span> 

1. <span data-ttu-id="1c3a5-232">Az Azure-kezelővel, kattintson a jobb gombbal a **Azure** gyökércsomópont, és válassza ki **előfizetések kezelése oldalt**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-232">In Azure Explorer, right-click the **Azure** root node, and then select **Manage Subscriptions**.</span></span> 

2. <span data-ttu-id="1c3a5-233">A párbeszédpanelen törölje az előfizetéseket, amelyet szeretne elérni, és jelölje be a jelölőnégyzeteket **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-233">In the dialog box, clear the check boxes next to the subscriptions that you don't want to access, and then select **Close**.</span></span> <span data-ttu-id="1c3a5-234">Igény szerint kiválaszthatja **Kijelentkezés** Ha azt szeretné, hogy kijelentkezik, az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-234">You can also select **Sign Out** if you want to sign out of your Azure subscription.</span></span>

## <a name="run-a-spark-scala-application-locally"></a><span data-ttu-id="1c3a5-235">A Spark Scala alkalmazás helyileg történő futtatása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-235">Run a Spark Scala application locally</span></span>
<span data-ttu-id="1c3a5-236">Az intellij-t Azure eszközkészlet segítségével Spark Scala-alkalmazások helyi futtatása a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-236">You can use Azure Toolkit for IntelliJ to run Spark Scala applications locally on your workstation.</span></span> <span data-ttu-id="1c3a5-237">Az alkalmazások általában nem szükséges hozzáférési jogot a fürterőforrások, például a tárolókban, és futtatja, és helyben tesztelheti őket.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-237">The applications usually don't need access to cluster resources, such as storage containers, and you can run and test them locally.</span></span>

### <a name="prerequisite"></a><span data-ttu-id="1c3a5-238">Előfeltétel</span><span class="sxs-lookup"><span data-stu-id="1c3a5-238">Prerequisite</span></span>
<span data-ttu-id="1c3a5-239">A helyi Spark Scala-alkalmazások Windows rendszerű számítógépeken futtatása, közben kivételt, kaphat, ahogy [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span><span class="sxs-lookup"><span data-stu-id="1c3a5-239">While you're running the local Spark Scala application on a Windows computer, you might get an exception, as explained in [SPARK-2356](https://issues.apache.org/jira/browse/SPARK-2356).</span></span> <span data-ttu-id="1c3a5-240">A kivétel történt, mert WinUtils.exe hiányzik a Windows.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-240">The exception occurs because WinUtils.exe is missing on Windows.</span></span> 

<span data-ttu-id="1c3a5-241">Ez a hiba megoldásához [töltse le a végrehajtható fájl](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) egy olyan helyre, például **C:\WinUtils\bin**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-241">To resolve this error, [download the executable](http://public-repo-1.hortonworks.com/hdp-win-alpha/winutils.exe) to a location such as **C:\WinUtils\bin**.</span></span> <span data-ttu-id="1c3a5-242">Adja hozzá a következő környezeti változó **HADOOP_HOME**, és állítsa be a változó értékének **C\WinUtils**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-242">Then, add the environment variable **HADOOP_HOME**, and set the value of the variable to **C\WinUtils**.</span></span>

### <a name="run-a-local-spark-scala-application"></a><span data-ttu-id="1c3a5-243">A helyi Spark Scala-alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-243">Run a local Spark Scala application</span></span>
1. <span data-ttu-id="1c3a5-244">Indítsa el az IntelliJ IDEA, és hozzon létre egy projektet.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-244">Start IntelliJ IDEA, and create a project.</span></span> 

2. <span data-ttu-id="1c3a5-245">Az a **új projekt** párbeszédpanelen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-245">In the **New Project** dialog box, do the following:</span></span>
   
    <span data-ttu-id="1c3a5-246">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-246">a.</span></span> <span data-ttu-id="1c3a5-247">Válassza ki **HDInsight** > **a Spark on HDInsight helyi futtatási minta (Scala)**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-247">Select **HDInsight** > **Spark on HDInsight Local Run Sample (Scala)**.</span></span>

    <span data-ttu-id="1c3a5-248">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-248">b.</span></span> <span data-ttu-id="1c3a5-249">Az a **Build eszköz** listára, válassza ki, az igényeknek megfelelően az alábbiak valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-249">In the **Build tool** list, select either of the following, according to your need:</span></span>

      * <span data-ttu-id="1c3a5-250">**Maven**, Scala-projekt létrehozása varázsló támogatásához</span><span class="sxs-lookup"><span data-stu-id="1c3a5-250">**Maven**, for Scala project-creation wizard support</span></span>
      * <span data-ttu-id="1c3a5-251">**SBT**, a függőségek kezelésére, és a Scala-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-251">**SBT**, for managing the dependencies and building for the Scala project</span></span>

    ![Az új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run.png)

3. <span data-ttu-id="1c3a5-253">Válassza ki **következő**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-253">Select **Next**.</span></span>
 
4. <span data-ttu-id="1c3a5-254">A következő ablakban tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-254">In the next window, do the following:</span></span>
   
    <span data-ttu-id="1c3a5-255">a.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-255">a.</span></span> <span data-ttu-id="1c3a5-256">Adja meg a projekt nevét és helyét.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-256">Enter a project name and location.</span></span>

    <span data-ttu-id="1c3a5-257">b.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-257">b.</span></span> <span data-ttu-id="1c3a5-258">Az a **projekt SDK** legördülő listára, válassza ki a 1.7 verziónál újabb Java-verziót.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-258">In the **Project SDK** drop-down list, select a Java version that's later than version 1.7.</span></span>

    <span data-ttu-id="1c3a5-259">c.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-259">c.</span></span> <span data-ttu-id="1c3a5-260">Az a **Spark verzió** legördülő listára, válassza ki a használni kívánt Scala verziója: Scala Spark 2.0-s vagy Scala 2.11.x 2.10.x a Spark 1.6-os.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-260">In the **Spark Version** drop-down list, select the version of Scala that you want to use: Scala 2.11.x for Spark 2.0 or Scala 2.10.x for Spark 1.6.</span></span>

    ![Az új projekt párbeszédpanel](./media/hdinsight-apache-spark-intellij-tool-plugin/Create-local-project.PNG)

5. <span data-ttu-id="1c3a5-262">Válassza a **Finish** (Befejezés) elemet.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-262">Select **Finish**.</span></span>

6. <span data-ttu-id="1c3a5-263">A sablon hozzáadása egy mintakód (**LogQuery**) alatt a **src** mappát, amelyet a számítógépen helyileg futtathat.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-263">The template adds a sample code (**LogQuery**) under the **src** folder that you can run locally on your computer.</span></span>
   
    ![LogQuery helye](./media/hdinsight-apache-spark-intellij-tool-plugin/local-app.png)

7. <span data-ttu-id="1c3a5-265">Kattintson a jobb gombbal a **LogQuery** alkalmazás, és adja **Futtatás "LogQuery"**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-265">Right-click the **LogQuery** application, and then select **Run 'LogQuery'**.</span></span> <span data-ttu-id="1c3a5-266">A a **futtatása** lap alján, a következő kimenetnek lásd:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-266">On the **Run** tab at the bottom, you see an output like the following:</span></span>
   
   ![Spark alkalmazás helyi futtatás eredménye](./media/hdinsight-apache-spark-intellij-tool-plugin/hdi-spark-app-local-run-result.png)

## <a name="convert-existing-intellij-idea-applications-to-use-azure-toolkit-for-intellij"></a><span data-ttu-id="1c3a5-268">Alakítsa át a meglévő IntelliJ IDEA alkalmazások az Azure-eszközkészlet az intellij-t</span><span class="sxs-lookup"><span data-stu-id="1c3a5-268">Convert existing IntelliJ IDEA applications to use Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="1c3a5-269">A meglévő Spark Scala használatával bármikor átalakítható létrehozott IntelliJ IDEA való kompatibilitás érdekében az IntelliJ Azure eszköztára alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-269">You can convert the existing Spark Scala applications that you created in IntelliJ IDEA to be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="1c3a5-270">Ezután használhatja a beépülő modul elküldeni az alkalmazásokat a HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-270">You can then use the plug-in to submit the applications to an HDInsight Spark cluster.</span></span>

1. <span data-ttu-id="1c3a5-271">Egy meglévő Spark Scala-alkalmazás, amely az IntelliJ IDEA használatával hozták létre nyissa meg a társított .iml fájlt.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-271">For an existing Spark Scala application that was created through IntelliJ IDEA, open the associated .iml file.</span></span>

2. <span data-ttu-id="1c3a5-272">Gyökerében szintje a **modul** elem a következő:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-272">At the root level is a **module** element like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4">

   <span data-ttu-id="1c3a5-273">Az elem hozzáadása szerkesztése `UniqueKey="HDInsightTool"` , hogy a **modul** elem a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-273">Edit the element to add `UniqueKey="HDInsightTool"` so that the **module** element looks like the following:</span></span>
   
        <module org.jetbrains.idea.maven.project.MavenProjectsManager.isMavenModule="true" type="JAVA_MODULE" version="4" UniqueKey="HDInsightTool">

3. <span data-ttu-id="1c3a5-274">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-274">Save the changes.</span></span> <span data-ttu-id="1c3a5-275">Az alkalmazás most már Azure eszköztára IntelliJ kompatibilisnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-275">Your application should now be compatible with Azure Toolkit for IntelliJ.</span></span> <span data-ttu-id="1c3a5-276">Kattintson a jobb gombbal a projekt nevére a Project Explorer tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-276">You can test it by right-clicking the project name in Project Explorer.</span></span> <span data-ttu-id="1c3a5-277">A legördülő menü most már a beállítás **küldje el a Spark-alkalmazást, amely**.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-277">The pop-up menu now has the option **Submit Spark Application to HDInsight**.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="1c3a5-278">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="1c3a5-278">Troubleshooting</span></span>

### <a name="error-in-local-run-please-use-a-larger-heap-size"></a><span data-ttu-id="1c3a5-279">Hiba történt a helyi futtatáskor: *használjon egy nagyobb halommemória mérete*</span><span class="sxs-lookup"><span data-stu-id="1c3a5-279">Error in local run: *Please use a larger heap size*</span></span>
<span data-ttu-id="1c3a5-280">A Spark 1.6 használata egy 32 bites Java SDK helyi futtatás során esetleg felmerülő hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-280">In Spark 1.6, if you're using a 32-bit Java SDK during local run, you might encounter the following errors:</span></span>

    Exception in thread "main" java.lang.IllegalArgumentException: System memory 259522560 must be at least 4.718592E8. Please use a larger heap size.
        at org.apache.spark.memory.UnifiedMemoryManager$.getMaxMemory(UnifiedMemoryManager.scala:193)
        at org.apache.spark.memory.UnifiedMemoryManager$.apply(UnifiedMemoryManager.scala:175)
        at org.apache.spark.SparkEnv$.create(SparkEnv.scala:354)
        at org.apache.spark.SparkEnv$.createDriverEnv(SparkEnv.scala:193)
        at org.apache.spark.SparkContext.createSparkEnv(SparkContext.scala:288)
        at org.apache.spark.SparkContext.<init>(SparkContext.scala:457)
        at LogQuery$.main(LogQuery.scala:53)
        at LogQuery.main(LogQuery.scala)
        at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
        at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
        at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
        at java.lang.reflect.Method.invoke(Method.java:606)
        at com.intellij.rt.execution.application.AppMain.main(AppMain.java:144)

<span data-ttu-id="1c3a5-281">Ezek a hibák akkor fordulhat elő, mert a halommemória mérete nem elég nagy Spark futtatásához.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-281">These errors happen because the heap size is not large enough for Spark to run.</span></span> <span data-ttu-id="1c3a5-282">Spark legalább 471 MB terület szükséges.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-282">Spark requires at least 471 MB.</span></span> <span data-ttu-id="1c3a5-283">(További információkért lásd: [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) Egy egyszerű megoldást, hogy egy 64 bites Java SDK-t használja.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-283">(For more information, see [SPARK-12081](https://issues.apache.org/jira/browse/SPARK-12081).) One simple solution is to use a 64-bit Java SDK.</span></span> <span data-ttu-id="1c3a5-284">Az IntelliJ JVM beállításait is módosíthatja adja hozzá a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="1c3a5-284">You can also change the JVM settings in IntelliJ by adding the following options:</span></span>

    -Xms128m -Xmx512m -XX:MaxPermSize=300m -ea

![Beállítások hozzáadása a "Virtuális gép beállításai" mezőben az intellij-t](./media/hdinsight-apache-spark-intellij-tool-plugin/change-heap-size.png)

## <a name="faq"></a><span data-ttu-id="1c3a5-286">GYIK</span><span class="sxs-lookup"><span data-stu-id="1c3a5-286">FAQ</span></span>
<span data-ttu-id="1c3a5-287">Azure Data Lake Store kérelmet elküldéséhez válassza **interaktív** mód az Azure bejelentkezési folyamat során.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-287">To submit an application to Azure Data Lake Store, choose **Interactive** mode during the Azure sign-in process.</span></span> <span data-ttu-id="1c3a5-288">Ha **automatikus** mód, hibaüzenetet kaphat.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-288">If you select **Automated** mode, you can get an error.</span></span>

![interaktív bejelentkezés](./media/hdinsight-apache-spark-intellij-tool-plugin/interative-signin.png)

<span data-ttu-id="1c3a5-290">Most azt feloldotta azt.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-290">Now, we resolved it.</span></span> <span data-ttu-id="1c3a5-291">Az Azure Data Lake fürt elküldeni az alkalmazás egyik bejelentkezési módszer kiválasztása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-291">You can choose an Azure Data Lake Cluster to submit your application with any sign-in method.</span></span>

## <a name="feedback-and-known-issues"></a><span data-ttu-id="1c3a5-292">Visszajelzések és ismert problémák</span><span class="sxs-lookup"><span data-stu-id="1c3a5-292">Feedback and known issues</span></span>
<span data-ttu-id="1c3a5-293">Spark kimenetek közvetlenül megtekintése jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-293">Currently, viewing Spark outputs directly is not supported.</span></span>

<span data-ttu-id="1c3a5-294">Ha javaslata vagy visszajelzést, vagy ha ez a beépülő modul használatakor bármely problémákat tapasztal, e-mail nekünk az hdivstool@microsoft.com.</span><span class="sxs-lookup"><span data-stu-id="1c3a5-294">If you have any suggestions or feedback, or if you encounter any problems when you use this plug-in, email us at hdivstool@microsoft.com.</span></span>

## <span data-ttu-id="1c3a5-295"><a name="seealso"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1c3a5-295"><a name="seealso"></a>Next steps</span></span>
* [<span data-ttu-id="1c3a5-296">Overview: Apache Spark on Azure HDInsight (Áttekintés: Apache Spark on Azure HDInsight)</span><span class="sxs-lookup"><span data-stu-id="1c3a5-296">Overview: Apache Spark on Azure HDInsight</span></span>](hdinsight-apache-spark-overview.md)

### <a name="demo"></a><span data-ttu-id="1c3a5-297">Bemutató</span><span class="sxs-lookup"><span data-stu-id="1c3a5-297">Demo</span></span>
* <span data-ttu-id="1c3a5-298">Hozzon létre Scala project (videó): [Spark Scala-alkalmazások létrehozása](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="1c3a5-298">Create Scala project (video): [Create Spark Scala Applications](https://channel9.msdn.com/Series/AzureDataLake/Create-Spark-Applications-with-the-Azure-Toolkit-for-IntelliJ)</span></span>
* <span data-ttu-id="1c3a5-299">Távoli hibakeresési (videó): [IntelliJ a Spark-alkalmazások távolról a HDInsight-fürt használata Azure eszköztára](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span><span class="sxs-lookup"><span data-stu-id="1c3a5-299">Remote debug (video): [Use Azure Toolkit for IntelliJ to debug Spark applications remotely on HDInsight Cluster](https://channel9.msdn.com/Series/AzureDataLake/Debug-HDInsight-Spark-Applications-with-Azure-Toolkit-for-IntelliJ)</span></span>

### <a name="scenarios"></a><span data-ttu-id="1c3a5-300">Forgatókönyvek</span><span class="sxs-lookup"><span data-stu-id="1c3a5-300">Scenarios</span></span>
* [<span data-ttu-id="1c3a5-301">Spark és BI: interaktív adatelemzés végrehajtása a Spark hdinsight BI-eszközökkel</span><span class="sxs-lookup"><span data-stu-id="1c3a5-301">Spark with BI: Perform interactive data analysis by using Spark in HDInsight with BI tools</span></span>](hdinsight-apache-spark-use-bi-tools.md)
* [<span data-ttu-id="1c3a5-302">Spark és Machine Learning: Spark on HDInsight HVAC-adatok épület-hőmérséklet elemzésére használata</span><span class="sxs-lookup"><span data-stu-id="1c3a5-302">Spark with Machine Learning: Use Spark in HDInsight to analyze building temperature using HVAC data</span></span>](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [<span data-ttu-id="1c3a5-303">Spark és Machine Learning: A Spark on HDInsight használata az élelmiszervizsgálati eredmények előrejelzésére</span><span class="sxs-lookup"><span data-stu-id="1c3a5-303">Spark with Machine Learning: Use Spark in HDInsight to predict food inspection results</span></span>](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [<span data-ttu-id="1c3a5-304">Spark Streaming: Spark on HDInsight használata valós idejű streamelési alkalmazásokat hozhatnak létre</span><span class="sxs-lookup"><span data-stu-id="1c3a5-304">Spark Streaming: Use Spark in HDInsight to build real-time streaming applications</span></span>](hdinsight-apache-spark-eventhub-streaming.md)
* [<span data-ttu-id="1c3a5-305">A webhelynapló elemzése a Spark on HDInsight használatával</span><span class="sxs-lookup"><span data-stu-id="1c3a5-305">Website log analysis using Spark in HDInsight</span></span>](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="creating-and-running-applications"></a><span data-ttu-id="1c3a5-306">Létrehozása és alkalmazások futtatása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-306">Creating and running applications</span></span>
* [<span data-ttu-id="1c3a5-307">Önálló alkalmazás létrehozása a Scala használatával</span><span class="sxs-lookup"><span data-stu-id="1c3a5-307">Create a standalone application using Scala</span></span>](hdinsight-apache-spark-create-standalone-application.md)
* [<span data-ttu-id="1c3a5-308">Feladatok távoli futtatása Spark-fürtön a Livy használatával</span><span class="sxs-lookup"><span data-stu-id="1c3a5-308">Run jobs remotely on a Spark cluster using Livy</span></span>](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a><span data-ttu-id="1c3a5-309">Eszközök és bővítmények</span><span class="sxs-lookup"><span data-stu-id="1c3a5-309">Tools and extensions</span></span>
* [<span data-ttu-id="1c3a5-310">Az intellij-t Azure eszközkészlet segítségével VPN-en keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="1c3a5-310">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through VPN</span></span>](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [<span data-ttu-id="1c3a5-311">Az intellij-t Azure eszközkészlet segítségével SSH keresztül távolról Spark-alkalmazások</span><span class="sxs-lookup"><span data-stu-id="1c3a5-311">Use Azure Toolkit for IntelliJ to debug Spark applications remotely through SSH</span></span>](hdinsight-apache-spark-intellij-tool-debug-remotely-through-ssh.md)
* [<span data-ttu-id="1c3a5-312">Használja a HDInsight Tools for IntelliJ a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="1c3a5-312">Use HDInsight Tools for IntelliJ with Hortonworks Sandbox</span></span>](hdinsight-tools-for-intellij-with-hortonworks-sandbox.md)
* [<span data-ttu-id="1c3a5-313">Az Eclipse Azure eszközkészlet a HDInsight Tools használatával Spark-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="1c3a5-313">Use HDInsight Tools in Azure Toolkit for Eclipse to create Spark applications</span></span>](hdinsight-apache-spark-eclipse-tool-plugin.md)
* [<span data-ttu-id="1c3a5-314">Zeppelin notebookok használata Spark-fürttel HDInsighton</span><span class="sxs-lookup"><span data-stu-id="1c3a5-314">Use Zeppelin notebooks with a Spark cluster on HDInsight</span></span>](hdinsight-apache-spark-zeppelin-notebook.md)
* [<span data-ttu-id="1c3a5-315">Jupyter notebookokhoz elérhető kernelek a HDInsight Spark-fürtjében</span><span class="sxs-lookup"><span data-stu-id="1c3a5-315">Kernels available for Jupyter notebook in Spark cluster for HDInsight</span></span>](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [<span data-ttu-id="1c3a5-316">Külső csomagok használata Jupyter notebookokkal</span><span class="sxs-lookup"><span data-stu-id="1c3a5-316">Use external packages with Jupyter notebooks</span></span>](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [<span data-ttu-id="1c3a5-317">A Jupyter telepítése a számítógépre, majd csatlakozás egy HDInsight Spark-fürthöz</span><span class="sxs-lookup"><span data-stu-id="1c3a5-317">Install Jupyter on your computer and connect to an HDInsight Spark cluster</span></span>](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="managing-resources"></a><span data-ttu-id="1c3a5-318">Erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="1c3a5-318">Managing resources</span></span>
* [<span data-ttu-id="1c3a5-319">Apache Spark-fürt erőforrásainak kezelése az Azure HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1c3a5-319">Manage resources for the Apache Spark cluster in Azure HDInsight</span></span>](hdinsight-apache-spark-resource-manager.md)
* [<span data-ttu-id="1c3a5-320">Apache Spark-fürtön futó feladatok nyomon követése és hibakeresése a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="1c3a5-320">Track and debug jobs running on an Apache Spark cluster in HDInsight</span></span>](hdinsight-apache-spark-job-debugging.md)

